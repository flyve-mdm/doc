API Endpoints Specifications
============================

Flyve MDM relies in GLPI's REST API for communications. The plugin provides additional endpoints for its itemtypes.

Agent
~~~~~

This endpoint allows the API consumer to read or modify agents.

* Name: PluginFlyvemdmAgent

File
~~~~

This endpoint allows the API consumer to read or modify files to deploy to devices. 

* Name: PluginFlyvemdmFile

Geolocation
~~~~~~~~~~~

This endpoint allows the API consumer to read or modify geographical positions acquired from GPS. 

* Name: PluginFlyvemdmGeolocation

Invitation
~~~~~~~~~~

This endpoint allows the API consumer to read or modify invitations. 

* Name: PluginFlyvemdmInvitation

InvitationLog
~~~~~~~~~~~~~

This endpoint allows the API consumer to read invitation events (errors only when a device failed to enroll). 

* Name: PluginFlyvemdmInvitationLog

Package
~~~~~~~

This endpoint allows the API consumer to read or modify packages (aka applications for mobile devices). 

* Name: PluginFlyvemdmPackage

Policy
~~~~~~

This endpoint allows the API consumer to read policies handled by Flyve MDM.

* Name: PluginFlyvemdmPolicy

PolicyCategory
~~~~~~~~~~~~~~

This endpoint allows the API consumer to read policy categories handled by Flyve MDM.

* Name: PluginFlyvemdmPolicyCategory

WellKnownPath
~~~~~~~~~~~~~

This endpoint allows the API consumer to read well known path in devices.

* Name: PluginFlyvemdmWellKnownPath