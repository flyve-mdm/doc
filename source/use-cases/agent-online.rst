Agent Online Status
===================

Flyve MDM takes benefit from the will and testament concept of MQTT protocol. When a client connects to the broker it can send a message to the broker. When the client goes offline, the broker sends the message on behalf of the disconnected client.

Online status
~~~~~~~~~~~~~

The online status relies on will and testament feature of MQTT. When a device goes online, it must send 

Sub topic ``/Status/Online``

.. code:: json
   
   {"online": "true"}

When the device disconnects it must set a will and testament message which will fire to the backend from the Broker on behalf of  the agent.

Sub topic ``/Status/Online``

.. code:: json

   {"online": "false"}

This way, when the Broker detects loss of connectivity with an agent, it will inform the backend that the devices are gone offline.
