UML Diagrams
============

Here we present you the sequence diagrams of the main interactions between the GLPI Plugin and Flyve MDM Agent.

Enrollment MQTT
---------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    participant U as User
    A->>P: create invitation to enroll with user email
    P->>P: create user in DB with given email
    P->>U: send invitation to user email account
    U-->>Ag: open invitation in the device, fill information
    Ag-->>P: send user information and inventory through HTTP request
    P->>P: update user account, add Agent to DB, create MQTT credentials
    P->>B: send MQTT credentials
    B->>Ag: send MQTT credentials
    Ag-->>B: connect to MQTT server

Enrollment FCM
--------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    participant U as User
    A->>P: create invitation to enroll with user email
    P->>P: create user in DB with given email
    P->>U: send invitation to user email account
    U-->>Ag: open invitation in the device
    Ag->>Ag: conect to FCM
    U-->>Ag: fill information
    Ag-->>P: send user information, FCM credentials and inventory through HTTP request
    P->>P: update user account, add Agent to DB, receive FCM credentials

Status Online/Offline MQTT
--------------------------

.. mermaid::

   sequenceDiagram
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    Ag->>B: connect to MQTT
    Ag->>Ag: update UI
    Ag->>B: last will status Offline
    B->>B: save last will message
    Ag-->>B: status Online
    B-->>P: status Online
    P->>P: update UI
    loop Maintain connection
        Ag->>B: Ping message
        B->>Ag: Ping response
    end
    Note right of Ag: For some reason, <br/>the agent looses connectivity
    Ag->>Ag: update UI
    Note right of B: Closing TCP socket
    B-->>P: status Offline
    P->>P: update UI

Status Online/Offline FCM
-------------------------

.. mermaid::

   sequenceDiagram
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    Ag->>B: connect to FCM
    Ag->>Ag: update UI
    Ag->>P:send through HTTP status Online
    P->>P: update UI

The Offline status is only sent to the backend when there is an unenrollment or wipe,
if the Agent looses connectivity in any other form, it won't send the status

Implementation of policies MQTT
-------------------------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    participant U as User
    alt fleet without Agent(s)
        A->>P: create fleet and set policies
        A->>P: assign agents to fleet
    else fleet with Agent(s)
        A->>P: add policy
    end
    opt device
        A->>P: set policy
    end
    P->>B: send policies individually
    B->>Ag: send policies individually
    Ag->>Ag: apply policies
    Ag-->>P: send task status through HTTP
    P->>P: update UI
    U->>Ag: change a setting controlled by a policy
    par get broadcast
        Ag->>Ag: check policy value
        Ag->>Ag: maintain policy value given by the Administrator
    end

Implementation of policies FCM
------------------------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    alt fleet without Agent(s)
        A->>P: create fleet and set policies
        A->>P: assign agents to fleet
    else fleet with Agent(s)
        A->>P: add policy
    end
    opt device
        A->>P: set policy
    end
    P->>B: send policies individually
    B->>Ag: send policies individually
    Ag->>Ag: apply policies
    Ag-->>P: send task status through HTTP
    P->>P: update UI

File & App download
-------------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    participant U as User
    A->>P: upload file/app
    A->>P: send download policy
    P->>B: send download policy
    B->>Ag: send download policy
    Ag-->>P: send HTTP request to download file
    Note right of Ag: For files the Agent has all priveleges <br/> to download them
    alt Agent has system privileges
        P->>Ag: send file/app through HTTP
        Ag->>Ag: download file/app
    else Agent doesn't have system privileges
        P->>Ag: send app through HTTP
        Note right of U: The request is available for API 23+ <br/> the user MUST enable unknown sources in minor versions <br/> to download the app 
        Ag->>U: request to allow unknown sources
        U-->>Ag: allow unknown sources
        Ag->>Ag: download app
    end
    Ag-->>P: send task status through HTTP
    P->>P: update UI

Ping request
------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    A->>P: send ping request
    P->>B: send ping request
    B->>Ag: send ping request
    Ag-->>P: ping response through HTTP
    P->>P: update UI

Geolocation
-----------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    A->>P: request Geolocation
    P->>B: request Geolocation
    B->>Ag: request Geolocation
    alt Agent answers immediately 
        Ag->>P: send status Geolocation
        P->>P: save Geolocation in DB
    else Agent doesn't answer immediately
        P->>P: display timeout error
    end

Wipe
----

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    A->>P: send wipe command
    P->>B: send wipe command
    B->>Ag: send wipe command
    Ag->>Ag: wipe phone data
    Ag-->>P: send task status and Offline status through HTTP
    P->>P: update UI

Lock/Unlock
-----------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    A->>P: send lock/unlock command
    P->>B: send lock/unlock command
    B->>Ag: send lock/unlock command
    Ag->>Ag: lock/unlock phone
    Ag-->>P: send task status through HTTP
    P->>P: update UI

Inventory
---------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    A->>P: request inventory
    P->>B: request inventory
    B->>Ag: request inventory
    Ag->>Ag: create inventory
    Ag-->>P: send inventory in XML format through HTTP
    P->>P: update UI

Unenrollment MQTT
-----------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    A->>P: send unenrollment
    P->>B: send unenrollment
    B->>Ag: send unenrollment
    Note right of Ag: Policies will have the last<br/> value sent by the Plugin,<br/> the user can now modify them
    Ag->>Ag: Update UI, remove constraint on policies
    Ag-->>B: disconnect MQTT
    Ag->>B: status unenrolled
    B-->>P: status unenrolled
    P->>P: remove user account, remove MQTT credentials
    alt removal successful
    P->>P: delete data related to the Agent, delete Agent from DB
    else removal unsuccessful
    P->>P: Agent remains in DB
    end

Unenrollment FCM
-----------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant B as Broker
    participant Ag as Agent
    A->>P: send unenrollment
    P->>B: send unenrollment
    B->>Ag: send unenrollment
    Note right of Ag: Policies will have the last<br/> value sent by the Plugin,<br/> the user can now modify them
    Ag->>Ag: Update UI, remove constraint on policies, remove FCM credentials
    Ag->>P: status unenrolled and status Offline
    P->>P: remove user account
    alt removal successful
    P->>P: delete data related to the Agent, delete Agent from DB
    else removal unsuccessful
    P->>P: Agent remains in DB
    end