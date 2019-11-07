<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

Pydio in-app upgrade tool allows you to directly migrate from an existing Community Edition installation to Pydio Enterprise Distribution. The steps are described below.

### Prepare for migration

There are some differences for running Pydio Enterprise Distribution (ED) compared to our core community package. Based on an End User License Agreement, some files of the ED are encrypted and require a license key to be used. 

- **IonCube Loaders:** Install on your server the php IonCube Loaders provided by IonCube [on their server](https://ioncube.com/loaders.php). Pick the right binary for your architecture. Beware that it depends on the OS, the architecture (32/64bits) and the PHP version. Their wizard provides you an easy way to install the loaders.
- **License Key:** Once installed, the ED will require a license name/license key pair that is available in your Pydio.com account once you have bought them to our sales services.
- **API Keys:** In order to connect to our protect packages repositories, you will need the API Keys provided in your pydio.com. These are NOT the same as the license name/license key pair.

### Set up upgrade channel

Go to **Menu > Logs & Other data > Upgrade ** :

- Change the update site to https://update.pydio.com/auth/
- Update the target channel to "Install Enterprise Distribution" : this will be for the migration only, after the operation make sure to switch back to **"Stable"** channel.
- In the Authenticated Update Site section, copy and paste the API Keys you took from your account. 

[:image-popup:1_installation_guide/migration/migrate_01_update_site.png]

Once you have saved the modification of the upgrade engine, click on "CHECK NOW". You should see a new package ready for installation: pydio-enterprise-migration-8.0.0.zip. If you have a connection error, like a '401 not authorized', make sure you correctly copied the API keys and try again.

[:image-popup:1_installation_guide/migration/migrate_02_update_available.png]

### Apply upgrade

You may now select the package, and apply upgrade.

[:image-popup:1_installation_guide/migration/migrate_03_select_package.png]

If everything goes well, you should see a welcome screen in the upgrade panel:

[:image-popup:1_installation_guide/migration/migrate_04_welcome.png]

### Set up license information

If you now reload the page fully, you should see an error message warning about the invalid license. Don't panic, this is normal. 

[:image-popup:1_installation_guide/migration/migrate_05_invalid_license.png]

In the left column menu, go to the newly appeared "Enterprise License" section, and there enter your License Name / License String in the form, then save. If you have some weird displays, please manually clear the caches of the application by connecting to your server and running the following command in your pydio installation folder, then reload.

    rm data/cache/plugins_*.*
    rm data/cache/i18n/*.*

[:image-popup:1_installation_guide/migration/migrate_06_setup_license.png]

Once the license is detected as valid, the panel should show the license information (number of users, expiration date)

[:image-popup:1_installation_guide/migration/migrate_07_license_valid.png]

### You're done!

Reloading the page, you should now have access to the new Enterprise Dashboard and the whole set of professional features provided by this distribution! 

[:image-popup:1_installation_guide/migration/migrate_08_finished.png]