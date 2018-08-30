Enrollment process
==================

Enrollment requests
~~~~~~~~~~~~~~~~~~~

The guest user enrolls himself using a QR code or a deeplink sent by email. They both contain 
the same useful data to build several HTTP requests.

The user token is a token to authenticate against the REST API. When an administrator invites someone to enroll his devices, 
an account is created. The login is the email address provided by the administrator.

The user API token is unique among all user accounts. Using it alone is sufficient to get a session, as described in GLPI documentation.

The invitation token is a hash with limited lifetime to identify the invitation being used at enrollment time.

After the enrollment the agent must connect to the MQTT broker. The serial of the device and the password sent 
by the backend in the enrollment process are the credentials.

Deeplink
~~~~~~~~

`Flyve MDM Deeplink <https://github.com/flyve-mdm/deeplink>`_, is a microservice hosted by us, that generates a deeplink with 
the information of the invitation to perform the enrollment of the device.

The organizations have the option to set up their own instance of Flyve MDM Deeplink, in order to manage by themselves this microservice.

Content
#######

The deeplink contains some base64 encoded semicolon separated fields like a CSV format:

* the URL of the backend for REST API requests
* a user API token belonging to a human user account
* an invitation token
* the name of the company's helpdesk
* the phone number of the company's helpdesk
* the website of the company's helpdesk
* the email of the company's helpdesk

All fields related to the helpdesk may not be populated by the administrators. The fields are ordered.

.. code :: http

   http://api.domain.com/deeplink/?data=aHR0cDovL2FwaS5kb21haW4uY29tLzs0NWVyamJ1ZGtscTU4NjVzZGtqaGprcztsa2hqZmtnc2RmNTQ2NjM0cztjb21wYW55J3MgaGVscGRlc2s7MDMzMTIzNDU2Nzg5O2h0dHBzOi8vc3VwcG9ydC5jb21wYW55LmNvbTtzdXBwb3J0QGNvbXBhbnkuY29t

Uses
#######

* Email account

When the invitation is opened from the email account of the user in the respective device to enroll, the email will 
display the deeplink with the information of the invitation encoded. Also the QR code image is attached.

After tapping the link, the MDM Agent will recognize it and start the enrollment.

* Computer browser

The deeplink can also be opened in a computer's browser, in this case the deeplink will decode part of the 
information and display the Helpdesk information and the QR code, so the user can scan it 
from the MDM Agent and make the enrollment.

* Mobile device browser

In case the invitation is opened with the browser of the device, it will display a button that contains 
the deeplink so the MDM Agent can recognize it and start the enrollment.

Obtaining a session token
~~~~~~~~~~~~~~~~~~~~~~~~~

The agent must first acquire a session token issuing a request like this:

.. code :: http

   GET http://api.domain.com/initSession?user_token=45erjbudklq5865sdkjhjks
   Content-Type: application/json

Note: **the header is required**

Answer:

.. code:: json

   200 OK
   {
     "session_token": "83af7e620c83a50a18d3eac2f6ed05a3ca0bea62"
   }

.. note ::

  The URL of the backend is the base for requests built by the client, which must be suffixed by the endpoint to reach. 
  See the `GLPI documentation <https://github.com/glpi-project/glpi/blob/master/apirest.md>`_ to know more about the rest API of GLPI.

Check the current profile
~~~~~~~~~~~~~~~~~~~~~~~~~

GLPI requires users have at least one profile to allow them to login. When users have several profiles, they need to send a request to switch between them. This may happen if a user enrolls his own device.

To ensure the device always use the guest profile of the user, the device must run this request

.. code:: http

   GET http://api.domain.com/getFullSession?session_token=83af7e620c83a50a18d3eac2f6ed05a3ca0bea62
   Content-Type: application/json

Note: **the header is required**

Answer (truncated):

.. code:: json

   200 OK
   {
     "glpi_plugins": {
       "1": "flyvemdm",
       "2": "fusioninventory"
     },

     "snip": "many content here, not shown",

     "glpiactiveprofile": {
        "id": 9,
       "name": "Flyve MDM guest users",
     },

     "snip": "many content here, not shown",

     "plugin_flyvemdm_guest_profiles_id": 9
    }

Many data are returned. The agent must read inside glpiactiveprofile the value **id**. This is the ID of the current profile. If it differs from the value of **plugin_flyvemdm_guest_profiles_id** then the device needs to change its profile. If they match, the device may skip the next request.

Switch to guest profile
#######################

.. code:: http

   POST http://api.domain.com/changeActiveProfile?profiles_id=9&session_token=83af7e620c83a50a18d3eac2f6ed05a3ca0bea62
   Content-Type: application/json

Note: **the header is required**

Note the value **profiles_id** in the query string. The value must match **plugin_flyvemdm_guest_ profiles_id** found in the previous request

Answer if the request fails
###########################

===========  ==============  =========
HTTP Status  Error Message   Cause
-----------  --------------  ---------
400          Bad Request     The request is probably malformed.
-----------  --------------  ---------
404          Item not found  The requested profile is not available or does not exists.
===========  ==============  =========

In both cases, the enrollment cannot continue.

Answer if the request succeeds
##############################

.. code:: json

   200 OK

Instantiation of the agent
~~~~~~~~~~~~~~~~~~~~~~~~~~

After a session token is acquired, the agent must create itself in the backend. It must send its serial number or its UUID. Sending both is recommended to avoid inventory problems.

.. code:: http

   POST http://api.domain.com/PluginFlyvemdmAgent?session_token=83af7e620c83a50a18d3eac2f6ed05a3ca0bea62
   Content-Type: application/json

Payload:

.. code:: json

   {"input":
   {
      "_email": "emailaddress@domain.com",
      "_invitation_token": "lkhjfkgsdf546634s",
      "_serial": "0123456ATDJ-045",
      "_uuid": "49D53434-0200-9D08-9000-01DEA9028055",
      "csr": "",
      "firstname": "my first name",
      "lastname": "my lastname",
      "version": "0.99.0",
      "type": "android"
    }
   }

* _email: email address of the invited user
* _invitation_token: invitation token found in  the invitation message
* _serial: serial number of the device (serial or uuid required, both is better)
* _uuid: UUID of the device (serial or uuid required, both is better)
* csr: certificate signing request (not implemented yet)
* firstname: first name of the user (optional)
* lastname: last name of the user (optional)
* version: version of the MDM agent
* type: type of MDM used (must be one of the MDM types supported by the backend)

Answer if the request fails
###########################

If the enrollment fails, a JSON array is returned

.. code:: json

   400 Bad Request
   [
      "ERROR_GLPI_ADD",
      "wrong email address"
   ]

The first string is a software error code.
The second string is an error message displayable to the user.

If the request fails, the enrollment cannot continue.

Answer if the request succeeds
##############################

.. code:: json

   201 OK
   {
      "id": "17",
      "message": "XML has been imported succesfully!"
   }

``id`` is the ID of the agent in the database. It is needed for the next request.

Get agent's initial settings (only if enrollment succeeded)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: http

   GET http://api.domain.com/PluginFlyvemdmAgent/17?session_token=83af7e620c83a50a18d3eac2f6ed05a3ca0bea62
   Content-Type: application/json


Answer if the request succeeds
##############################

.. code:: json

   200 OK
   {
     "id": 65,
     "name": "user5@teclib.com",
     "version": "1.0.0",
     "computers_id": 65,
     "wipe": 0,
     "lock": 0,
     "enroll_status": "enrolled",
     "entities_id": 76,
     "plugin_flyvemdm_fleets_id": 103,
     "last_report": null,
     "last_contact": null,
     "certificate": "",
     "topic": "/76/agent/AZERTY",
     "mqttpasswd": "YTQUJagG7iBas45thrbwDxtzaQxZIhp4g",
     "api_token": "2qanibb1aoi16d7edqm6wcl14y6f3c24vtjupk4r"
     "broker": "demo.flyve.org",
     "port": 8883,
     "tls": 1,
     "android_bugcollecctor_url": "",
     "android_bugcollector_login": "",
     "android_bugcollector_passwd": "",
     "links": [
       {
         "rel": "Computer",
         "href": "https://demo.flyve.org/glpi/apirest.php/Computer/65"
       },
       {
         "rel": "Entity",
         "href": "https://demo.flyve.org/glpi/apirest.php/Entity/76"
       },
       {
         "rel": "PluginFlyvemdmFleet",
         "href": "https://demo.flyve.org/glpi/apirest.php/PluginFlyvemdmFleet/103"
       }
     ]
   }

.. note::
 
   The property certificate in the JSON payload is the certificate delivered to the agent if the client certificate feature is enabled.

* **api_token**: to consume API from GLPI. Used to download files and applications from HTTP(S).
* **mqttpasswd**: password to access MQTT. Login is the serial of the device.

The api_token delivered by this request replaces the user_token used in the first request **initSession**. The agent must forget the user_token and save for later use the api_token received from this request.

Logout after enrollment
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: http

   GET http://api.domain.com/killSession?session_token=83af7e620c83a50a18d3eac2f6ed05a3ca0bea62
   Content-Type: application/json

The answer should contain an empty body.

Answer if the request succeeds
##############################

.. code:: json

   200 OK

Answer if the request fails
###########################

.. code:: json

   400 Bad Request