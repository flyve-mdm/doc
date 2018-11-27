UML Diagrams
============

Here we present you the sequence diagrams of the main interactions between the GLPI Plugin and Flyve MDM Agent.

Enrollment
----------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant M as Mosquitto
    participant Ag as Agent
    participant U as User
    A->>P: create invitation to enroll with user email
    P->>P: create user in DB with given email
    P->>U: send invitation to user email account
    U-->>Ag: open invitation in the device, fill information
    Ag-->>P: send user information and inventory through HTTP request
    P->>P: update user account, add Agent to DB, create MQTT credentials
    P->>M: send MQTT credentials
    M->>Ag: send MQTT credentials
    Ag-->>M: connect to MQTT server

Status Online/Offline
---------------------

.. mermaid::

   sequenceDiagram
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    Ag->>M: connect to MQTT
    Ag->>Ag: update UI
    Ag->>M: last will status Offline
    M->>M: save last will message
    Ag-->>M: status Online
    M-->>D: status Online
    D-->>P: status Online
    P->>P: update UI
    loop Maintain connection
        Ag->>M: Ping message
        M->>Ag: Ping response
    end
    Note right of Ag: For some reason, <br/>the agent looses connectivity
    Ag->>Ag: update UI
    Note right of M: Closing TCP socket
    M-->>D: status Offline
    D-->>P: status Offline
    P->>P: update UI

Implementation of policies
--------------------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
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
    P->>M: send policies individually
    M->>Ag: send policies individually
    Ag->>Ag: apply policies
    Ag-->>M: send task status
    M-->>D: send task status
    D-->>P: send task status
    P->>P: update UI
    U->>Ag: change a setting controlled by a policy
    par get broadcast
        Ag->>Ag: check policy value
        Ag->>Ag: maintain policy value given by the Administrator
    end

File & App download
-------------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    participant U as User
    A->>P: upload file/app
    A->>P: send download policy
    P->>M: send download policy
    M->>Ag: send download policy
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
    Ag-->>M: send task status
    M-->>D: send task status
    D-->>P: send task status
    P->>P: update UI

Ping request
------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    A->>P: send ping request
    P->>M: send ping request
    M->>Ag: send ping request
    Ag-->>M: ping response
    M-->>D: ping response
    D-->>P: ping response
    P->>P: update UI

Geolocation
-----------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    A->>P: request Geolocation
    P->>M: request Geolocation
    M->>Ag: request Geolocation
    alt Agent answers immediately 
        Ag->>M: send status Geolocation
        M-->>D: send status Geolocation
        D-->>P: send status Geolocation
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
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    A->>P: send wipe command
    P->>M: send wipe command
    M->>Ag: send wipe command
    Ag->>Ag: wipe phone data
    Ag-->>M: send task status
    M-->>D: send task status
    D-->>P: send task status
    P->>P: update UI

Lock/Unlock
-----------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    A->>P: send lock/unlock command
    P->>M: send lock/unlock command
    M->>Ag: send lock/unlock command
    Ag->>Ag: lock/unlock phone
    Ag-->>M: send task status
    M-->>D: send task status
    D-->>P: send task status
    P->>P: update UI

Inventory
---------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    A->>P: request inventory
    P->>M: request inventory
    M->>Ag: request inventory
    Ag->>Ag: create inventory
    Ag-->>M: send inventory in XML format
    M-->>D: send inventory in XML format
    D-->>P: send inventory in XML format
    P->>P: update UI

Unenrollment
------------

.. mermaid::

   sequenceDiagram
    participant A as Administrator
    participant P as Plugin
    participant D as Daemon
    participant M as Mosquitto
    participant Ag as Agent
    A->>P: send unenrollment
    P->>M: send unenrollment
    M->>Ag: send unenrollment
    Note right of Ag: Policies will have the last<br/> value sent by the Plugin,<br/> the user can now modify them
    Ag->>Ag: Update UI, remove constraint on policies
    Ag-->>M: disconnect MQTT
    Ag->>M: status unenrolled
    M-->>D: status unenrolled
    D-->>P: status unenrolled
    P->>P: remove user account, remove MQTT credentials
    alt removal successful
    P->>P: delete data related to the Agent, delete Agent from DB
    else removal unsuccessful
    P->>P: Agent remains in DB
    end