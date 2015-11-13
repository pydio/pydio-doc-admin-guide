Deploy Pydio in a couple of steps :

1. Download and deploy archive on your server
2. Run through the diagnostic and fix your server configurations
3. Run through the installer to be up and running quickly
4. Use Pydio.

### Deploy archive on your server

Download the latest stable version from [https://pydio.com/en/get-pydio Download Page], either as ZIP or TAR.GZ format. Alternatively, you can use our Linux repositories to install Pydio using a package manager on Debian or CentOS Linux flavors.

Using your favorite FTP client or SCP command, upload this package to your webserver, and extract its content to a web-accessible folder (e.g. */var/www/pydio*).

Make sure the *data* path is writeable by the webserver. For example :

> `chown -R www-data:www-data /var/www/pydio/data`

### Run through the diagnostic and fix the necessary errors

At very first run, a diagnostic tool will check a couple of features and configurations of your server, and will report errors or warning as it find them, along with suggestions for fixing the issues.

[:image-popup:quick_start/00-DIAGNOSTIC.png]

While **“Warnings”** can be generally ignored without preventing the application from running, you should still try to fix them if you can, as you will surely encounter problems at one point or another.

**“Error”** failing tests must be fixed before going further, otherwise the application will very surely crash.

Once you think you have correctly fixed the issue, simply reload the page to see if the test now correctly passes. If you ever need to re-run this test page later, you can access it by calling runTests.php at the root of your Pydio install path. However, for security reasons, you will first have to edit this file manually and comment out the first line of code :

> `// die("You are not allowed to see this page ...")`

See the How-To’s for an exhaustive list of tests and solutions.

### Pydio provides you a GUI-based installer that will guide you through the basic configurations which is cut in three part.

- **Main Options: Five parameters here:**
    + **Application Title: The title to display in the tab.**
    + **Welcome Message: An additional message displayed at startup, on the splash screen and the login screen.**
    + **Administrator Login: The administrator login to connect to Pydio.**
    + **Administrator Full Name: Full name of the administrator.**
    + **Administrator Password: We recommend you have capital letters and special characters in your password.**

[:image-popup:quick_start/01-MAIN-OPTIONS.png]

    **Configuration Storage**: defines a basic storage type for all Pydio configuration data. These are the internal application data (users definitions, workspaces parameters, etc), not the actual business data (your files).
    Use a DB-based storage, the no-db is still active for backward compatibiliy but should be considered as deprecated.  You can use a MySQL, PostgreSQL or Sqlite server. The latter is a good option for a quick start, as it will create a simple in-file DB in your Pydio data folder.

[:image-popup:quick_start/02-DATABASE-CONNECTON.png]

- **Advanced Options: Two major parameters here:**
    + **Encoding**: the “Encoding” parameter must be correctly set otherwise you will very probably bump into problems if you upload files with names containing non-ascii characters (like french èé, german ü, etc;…).
    On a Linux install, the detected value will generally display an ENCODING (like UTF-8), but make sure to set it with a LOCALE.ENCODING value, like en_US.UTF-8, or fr_FR.UTF-8, etc… Login to your server and type ‘echo $LANG’ in a console and copy the result.
    Windows should be left empty.
    + **Server Path**: the server path should be automatically detected by the installer, but make sure that it’s correct. It is the path of Pydio install from the webroot. E.g if Pydio is installed on https://yourdomain.com/path/to/pydio , must be /path/to/pydio. This is important as it is used for the rewrite rules.

[:image-popup:quick_start/03-ADVANCED-OPTIONS.png]

Once you are ready to go, click on “Install Pydio Now” and the page should refresh and provide you a login dialog.
After the installation is complete, you should have a page like this :

[:image-popup:quick_start/04-FINISH.png]

Use the admin login you just defined to enter. You’re in!