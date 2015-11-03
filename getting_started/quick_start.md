Deploy Pydio in a couple of steps :

1. Download and deploy archive on your server
2. Run through the diagnostic and fix your server configurations
3. Run through the installer to be up and running quickly
4. Use Pydio.

### Deploy archive on your server
Download the latest stable version from [https://pyd.io/download Download Page], either as ZIP or TAR.GZ format.

Using your favorite FTP client or SCP command, upload this package to your webserver, and extract its content to a web-accessible folder (e.g. **/var/www/pydio**).

Make sure the **_data_** path is writeable by the webserver. For example :

> `chown -R www-data:www-data /var/www/pydio/data`

### Run through the diagnostic and fix the necessary errors
At very first run, a diagnostic tool will check a couple of features and configurations of your server, and will report errors or warning as it find them, along with suggestions for fixing the issues.

[:image-popup:getting_started/quick_start/screenshot-2013-05-08-at-12-50-32.png]

While “**Warnings**” can be generally ignored without preventing the application from running, you should still try to fix them if you can, as you will surely encounter problems at one point or another. Typically, the “Encoding” warning tells you that the application does not correctly recognize the server encoding setting, and if you skip that, you will surely bump into problems if you upload files with names containing non-ascii characters (like french èé, german ü, etc;…).

“**Error**” failing tests must be fixed before going further, otherwise the application will very surely crash.

Once you think you have correctly fixed the issue, simply reload the page to see if the test now correctly passes. If you ever need to re-run this test page later, you can access it by calling runTests.php at the root of your Pydio install path. However, for security reasons, you will first have to edit this file manually and comment out the first line of code :

> `// die("You are not allowed to see this page ...")`

See the Knowledge Base for an exhaustive list of tests and solutions.

TODO : add link to KB

### Run through the installer
In the new v5, Pydio provides you a GUI-based installer to simplify first run. The two major mandatory informations that you must enter before going further are the following :

+ **Admin credentials** : contratry to previous version, where an admin/admin was created by default, you will manually define the user name and password that will be created with administrator rights.
+ **Configuration Storage** : here you will define a basic storage type for all Pydio configurations. These are all the internal application data (users definitions, workspaces parameters, etc), NOT the actual business data storage (your files), these are configured for each workspaces.
	- **No-DB** : this is best if you want a fast installation, don’t want to setup a database, and you know that you won’t have a huge amount of users (less than 50). All configurations will be stored on the file system, which is handy to be set up and running in no time, but will have some drawbacks : some features won’t be available (notifications for example), and this configuration does not scale very well.
	- **DB-based** : best for performances and data integrity. Currently **_MySQL_** and **_Sqlite_** are supported. The latter is also a quick solution for being set up and running fast, but MySQL should be preferred. You will have to provide the standard connexion informations : for MySQL, host/database name and credentials, for SQLite the filename. If you select this configuration, all SQL tables will be automatically installed in the database.

[:image-popup:getting_started/quick_start/screenshot-2013-05-08-at-12-53-55.png]
 

Additional settings can be configured directly, as well as directly defining non-admin users. All these can be modified afterward by switching to the application Settings panel.

Once you are ready to go, click on “Install Pydio Now” and the page should refresh and provide you a login dialog. Use the admin login you just defined to enter. You’re in!