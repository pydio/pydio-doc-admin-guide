###Upgrading from v5.2.5
Upgrade is automatic from within the app for archive-based installs (zip / tar.gz). Make sure to backup the database and the files.

[:image-popup:1_getting_started/getting_started_update_plugin_parameters.png]

For update to v6 from stable version 5.2.5, please be sure that you have configured action.updater as following capture:

[:image-popup:1_getting_started/getting_started_update_dialog.png]

After saving the modification, click on upgrade button on top right bar, you can see this dialog.

[:image-popup:1_getting_started/getting_started_after_upgrade.png]

Click on Start Upgrade on dialog

If there is no error reported, You can logout, refresh the page login again and experience new version

### v4.0 and higher : automatic upgrades
[:image-popup:1_getting_started/getting_started_automatic_upgrade.png] Upgrade ButtonAs for v4.0 and next, simply select the automatic upgrade button in the toolbar and follow the procedure. Basically, you just have to make sure that your installation is writeable by the server at the time of the upgrade.

### Upgrade from 3.2.4 to 4.0
There is a specific « Import Tool » developed in v4.X to import v3.2.X data. It is not activated by default to avoid polluting the GUI. It’s not an « upgrade » but an « import » tool : **you must install the v4 in a different folder, and import the old data from the 3.2.4 to the new 4.X version.**

+ To enable the tool, go to Settings > Global Configs > Plugins > Action > Updater Engine : open the plugin configs, set the « Enable 3.2.4 Import tool » option to « Yes », and save.
+ Refresh the GUI (or switch back and forth to/from another repository), you will see a second button next to the standard « Upgrade » button, in the toolbar (top right).
+ Click on « Import 3.2.4″ button and follow the procedure. It’s is done a first time as « dry-run » to show you all operations that will be done, and then it is really executed. Your initial 3.2.4 install will stay untouched.
+ Once done, do not forget to clear the server cache (data/cache) and hard refresh the client (Ctrl+F5).

[**UPDATE**] If you encounter the « Invalid argument supplied for foreach » error while importing, check the forum : https://pyd.io/vanilla/discussion/2669/solved-3.2.4-to-4.0-upgrading-error-invalid-argument-supplied-for-foreach/p1

### Upgrade to 3.2.4 from below
In standard installation (no specific customization except configuration), your specific configuration data can be found in server/conf/ and server/users. On this base, the upgrade should be quite straightforward :

+ Download and install AjaXplorer inside a new folder
+ Copy the whole content of the old server/users/ folder inside the new server/users/
+ Do the same with the old server/logs/ folder content
+ If you had not made any changes to server/conf/conf.php, this step is not necessary. Otherwise, open the old and new server/conf/conf.php and compare them. Copy your changes to the new file.
+ Copy the file server/conf/repo.ser inside the new server/conf/ folder.
+ If the files accessed by your repositories where inside the ajaxplorer installation (which is not recommended), you have to copy them to the new location, and you’ll have to re-check the PATH parameter of your repositories. [A tip: working on a remote server, if you don’t have an ssh access, use AjaXplorer to make this move, as it can be quite long if you have to use ftp…]
+ Access your new install. You may see the diagnostic page, as the the diagnostic results are generated at install. Go forward and you should be able to login with the previous users rights and repositories.
+ If you had specific plugins that need external downloads, do not forget to update them (CKEditor distribution, OpenLayer distribution)
