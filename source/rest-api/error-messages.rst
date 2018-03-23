Error Messages
==============

GLPI
----

* initSession

===========  =====================================  =============
HTTP Status  Error Symbol                           Error Message
-----------  -------------------------------------  -------------
200
-----------  -------------------------------------  -------------
400          ERROR_LOGIN_PARAMETERS_MISSING         Parameter(s) login, password or user_token missing
-----------  -------------------------------------  -------------
400          ERROR_LOGIN_WITH_CREDENTIALS_DISABLED  initSession with credentials is disabled
-----------  -------------------------------------  -------------
401          ERROR_GLPI_LOGIN                       Parameter user_token seems invalid
===========  =====================================  =============

* killSession

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* changeActiveEntities

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* getMyEntities

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* getActiveEntities

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* changeActiveProfile

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* getMyProfiles

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* getActiveProfile

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* getFullSession

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
200
===========  ==============  =============

* getItem

===========  ============  =============
HTTP Status  Error Symbol  Error Message
-----------  ------------  -------------
200
-----------  ------------  -------------
401                        You don't have permission to perform this action.
===========  ============  =============

* getItems

===========  =======================================  =============
HTTP Status  Error Symbol                             Error Message
-----------  ---------------------------------------  -------------
200
-----------  ---------------------------------------  -------------
400          ERROR_ITEMTYPE_NOT_FOUND_NOR_COMMONDBTM  Parent itemtype is not found or an instance of CommonDBTM
===========  =======================================  =============

* listSearchOptions

===========  =======================================  =============
HTTP Status  Error Symbol                             Error Message
-----------  ---------------------------------------  -------------
200
-----------  ---------------------------------------  -------------
400          ERROR_ITEMTYPE_NOT_FOUND_NOR_COMMONDBTM  Parent itemtype is not found or an instance of CommonDBTM
===========  =======================================  =============

* searchItems

===========  ========================  =============
HTTP Status  Error Symbol              Error Message
-----------  ------------------------  -------------
200
-----------  ------------------------  -------------
400          ERROR_RANGE_EXCEED_TOTAL  Provided range exceed total count of data: ``placeholder``
-----------  ------------------------  -------------
500          ERROR_SQL                 Unexpected SQL Error: ``placeholder``
===========  ========================  =============

* createItems

===========  ======================  =============
HTTP Status  Error Symbol            Error Message
-----------  ----------------------  -------------
200
-----------  ----------------------  -------------
207          ERROR_GLPI_PARTIAL_ADD  
-----------  ----------------------  -------------
400          ERROR_GLPI_ADD          
===========  ======================  =============

* updateItems

===========  =========================  =============
HTTP Status  Error Symbol               Error Message
-----------  -------------------------  -------------
200
-----------  -------------------------  -------------
207          ERROR_GLPI_PARTIAL_UPDATE  
-----------  -------------------------  -------------
400          ERROR_GLPI_UPDATE         
===========  =========================  =============

* deleteItems

===========  =========================  =============
HTTP Status  Error Symbol               Error Message
-----------  -------------------------  -------------
200
-----------  -------------------------  -------------
207          ERROR_GLPI_PARTIAL_DELETE  
-----------  -------------------------  -------------
400          ERROR_GLPI_DELETE         
===========  =========================  =============

Flyve MDM
---------

All itemtypes may produce the error messages from GLPI, depending on their HTTP verb. Additional errors messages are below.

PluginFlyvemdmAgent
~~~~~~~~~~~~~~~~~~~

* addItem

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Unable to find an enrollment method
===========  ==============  =============

* updateItem

===========  =================  =============
HTTP Status  Error Symbol       Error Message
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  The fleet of the device does not longer exist
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  The target fleet does not exist
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  The device is not enrolled yet
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Timeout querying the device
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Timeout querying the device inventory
===========  =================  =============

* deleteItem

===========  =================  =============
HTTP Status  Error Symbol       Error Message
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Failed to find the guest user profile
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Failed to remove guest habilitation for the user of the device
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Failed to delete the device
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Failed to delete documents attached to the device
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Timeout querying the device inventory
===========  =================  =============

PluginFlyvemdmFile
~~~~~~~~~~~~~~~~~~

* addItem

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Could not upload file
===========  ==============  =============

* updateItem

===========  =================  =============
HTTP Status  Error Symbol       Error Message
-----------  -----------------  -------------
200	     ERROR_GLPI_UPDATE  File successfully uploaded
-----------  -----------------  -------------
207          ERROR_GLPI_UPDATE  Could not upload file
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Could not save file
===========  =================  =============

PluginFlyvemdmFleet_Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~

* addItem

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Fleet and policy must be specified
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Policy not found
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Incorrect value for this policy
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Cannot find the target fleet
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Cannot apply a policy on a not managed fleet
-----------  --------------  -------------
400          ERROR_GLPI_ADD  The requirements for this policy are not met
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Failed to apply the policy
===========  ==============  =============

* updateItem

===========  =================  =============
HTTP Status  Error Symbol       Error Message
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Policy not found
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Incorrect value for this policy
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Cannot find the target fleet
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Cannot apply a policy on a not managed fleet
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  The requirements for this policy are not met
-----------  -----------------  -------------
400          ERROR_GLPI_UPDATE  Failed to apply the policy
===========  =================  =============

* deleteItem

===========  =================  =============
HTTP Status  Error Symbol       Error Message
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Policy not found
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Fleet not found
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Incorrect value for this policy
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Cannot find the target fleet
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Cannot apply a policy on a not managed fleet
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  The requirements for this policy are not met
-----------  -----------------  -------------
400          ERROR_GLPI_DELETE  Failed to apply the policy
===========  =================  =============

Flyve MDM Demo plugin
---------------------

PluginFlyvemdmDemoUser
~~~~~~~~~~~~~~~~~~~~~~

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Email address is not valid
-----------  --------------  -------------
400          ERROR_GLPI_ADD  No inactive profile available to setup user rights
-----------  --------------  -------------
400          ERROR_GLPI_ADD  An entity already exists for your email. You probably already have an account
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Failed to create default fleet
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Passwords are different
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Password too short
===========  ==============  =============

PluginFlyvemdmdemoAccountvalidation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

===========  ==============  =============
HTTP Status  Error Symbol    Error Message
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Validation token missing
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Validation token is invalid
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Validation token expired
-----------  --------------  -------------
400          ERROR_GLPI_ADD  Failed to find your account 
===========  ==============  =============
