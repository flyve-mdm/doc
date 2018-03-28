Agent deletion
==============

There are two cases where an Agent must be deleted from the database:

* After succesful enrollment
* After explicit deletion by an administrator

Unenrollment
~~~~~~~~~~~~

When an administrator unenrolls an agent, a MQTT message is sent to the device running this agent to process unenrollment. Once the unenrollment is done, the agent sends a MQTT message to acknowledge the unenrollment request.

The backend waits for this acknowledgement to actually delete the agent from the database. The delay to send this acknowledgement is undertermined but should happen because the device is in the end of a cycle but still owned by legitimate user and is used in normal operations.

Forced deletion
~~~~~~~~~~~~~~~

When an enrolled device is lost or stolen, and it appears impossible to retrieve it, the administrator may want to delete its data from the database. In such case we cannot unenroll th device as it is very likely offline forever. We need to delete the agent and maybe the associated computer by force.

To do this the administrator deletes the agent fron his user interface. Next he is free to delete or keep the associated computer. This means that deleting the agent does not cascade a computer deletion. This is an extra and manual action taken by the administrator.