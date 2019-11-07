<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

## In-App upgrade from Pydio 6

Upgrade is automatic from within the app for archive-based installs (zip / tar.gz). Make sure to backup the database and the files.

[:image-popup:1_installation_guide/getting_started_update_plugin_parameters.png]

For update to v6 from stable version 5.2.5, please be sure that you have configured action.updater as following capture:

[:image-popup:1_installation_guide/getting_started_update_dialog.png]

After saving the modification, click on upgrade button on top right bar, you can see this dialog.

[:image-popup:1_installation_guide/getting_started_after_upgrade.png]

Click on Start Upgrade on dialog

If there is no error reported, You can logout, refresh the page login again and experience new version

## Upgrading from Pydio 6 when installed via apt/yum

If Pydio is installed via a package manager, you can use the in-app upgrade processus. You first have to update the packages, using yum / apt-get commands, then manually apply the followings steps:
 
 - Update database
 - Check /etc/bootstrap_*.php are up-to-date
 - Update share.php

## Upgrading from previous versions

Please read the upgrade instructions of v6 documentation.