Update
======

Remember to:

* Check the compatibility matrix of your GLPI version with Flyve MDM Plugin before any update.
* Make a backup of your database.
* Make a backup of the files of GLPI.

Compatibility Matrix
--------------------

=================  =======  ======== =======
GLPI               9.1.x      9.2.x  9.3.x
-----------------  -------  -------- -------
Flyve MDM          1.x.x    2.0.0    2.0.0
-----------------  -------  -------- -------
FusionInventory    9.1+1.0  9.2+1.0  9.3+1.0
-----------------  -------  -------- -------
Web MDM Dashboard     \-    2.0.0    2.0.0
=================  =======  ======== =======

From one release to another
---------------------------

* Download the new version of the plugin.
* Go to glpi/plugins directory.
* Move the folder of Flyve MDM plugin out of the GLPI subtree (old version).
* Install the new version of the plugin (this one should be named flyvemdm).
* Change the current directory to the content of Flyve MDM plugin ``cd glpi/plugins/flyvemdm``.
* Run ``php tools/cli_install.php`` in order to update your database.
* Enable the plugin again with the user interface of GLPI.
* Check in the file glpi/plugins/flyvemdm/scripts/service.sh that the user of the daemon is the same as your HTTP server (www-data for debian based systems).
* If you need to change the user, please run ``update-rc.d flyvemdm defaults``.
* Restart the daemon ``service flyvemdm restart``.

After you successfully confirm everything is as it should, delete the old version you moved out of GLPI.

Using Git
---------

If you installed the Flyve MDM plugin through git:

* Go to the Flyve MDM plugin's directory.
* Use ``git pull`` to update your local branch.
* Run ``php tools/cli_install.php`` to update the database.
* Enable the plugin again with the user interface of GLPI.
* Check in the file glpi/plugins/flyvemdm/scripts/service.sh that the user of the daemon is the same as your HTTP server (www-data for debian based systems).
* If you need to change the user, please run ``update-rc.d flyvemdm defaults``
* Restart the daemon ``service flyvemdm restart``.

You can use this method to upgrade the plugin from a revision to another revision on the develop branch.