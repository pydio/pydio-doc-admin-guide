<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

Deploy Pydio in a couple of steps :

1. Run through the diagnostic and fix your server configurations
2. Run through the installer to be up and running quickly
3. Use Pydio.

### Run through the diagnostic and fix the necessary errors

At very first run, a diagnostic tool will check a couple of features and configurations of your server, and will report errors or warning as it find them, along with suggestions for fixing the issues.

[:image-popup:1_installation_guide/getting_started_diagnostic.png]

While **“Warnings”** can be generally ignored without preventing the application from running, you should still try to fix them if you can, as you will surely encounter problems at one point or another.

**“Error”** failing tests must be fixed before going further, otherwise the application will very surely crash.


Once you think you have correctly fixed the issue, simply reload the page to see if the test now correctly passes. See the **[Troubleshooting](https://pydio.com/en/docs/v8/troubleshooting)** section for an exhaustive list of tests and solutions.

### Pydio Installation Wizard

After the diagnostic tool, an easy wizard will guide you through the configuration of the platform. Make sure to gather the necessary information before starting: create a database and prepare its credentials.

[:image-popup:1_installation_guide/installer/01-welcome.png]

#### Global Settings and admin user

Set up a title for your installation and a welcome message for your users.

[:image-popup:1_installation_guide/installer/02-settings.png]

Choose an identifier and a password for your administrative user, and eventually set up it's display name that will be shown to other users.

[:image-popup:1_installation_guide/installer/03-authentication.png]

#### Database Access

In this section, define the database driver and access. MySQL and PostgreSQL are fitted for production, Sqlite3 is definitely not, use it for quick testing only. If you are using MariaDB, simply select MySQL.

[:image-popup:1_installation_guide/installer/04-database.png]

#### Advanced Options

- **Server Path:** the server path should be automatically detected by the installer, but make sure that it’s correct. This is important as it is used for to generate correct rewrite rules.
- **Encoding:** the “Encoding” parameter must be correctly set otherwise you will very probably bump into problems if you upload files with names containing non-ascii characters (like french èé, german ü, etc;…). On a Linux install, the detected value will generally display an ENCODING (like UTF-8), but make sure to set it with a LOCALE.ENCODING value, like en_US.UTF-8, or fr_FR.UTF-8, etc… Login to your server and type ‘echo $LANG’ in a console and copy the result. Windows should be left empty.
- **Cache:** If you have installed APC/APCu php extension, this should be enabled automatically. If not, you can manage this setting later in the administration board.
- **Mailer:** If your php has mail or sendmail correctly configured, you can directly configure and test it from there.
- **Default Language:** Default language for the interface, users can select their own language anyway.

[:image-popup:1_installation_guide/installer/05-advanced.png]

#### [ED] License Setup

For Enterprise Distribution only, you can set up the license name / license key that was provided to you via your pydio.com account. If you don't have any, leave values empty to start a 30-days trial. This step is not present in the Community Edition.

[:image-popup:1_installation_guide/installer/06-license.png]

#### [ED] EULA

Pydio Enterprise Distribution requires you to agree with the terms of the End Users License Agreement. This step is not present in the Community Edition.

[:image-popup:1_installation_guide/installer/07-EULA.png]

#### Finish

Once you are ready to go, click on “Install Pydio” and the page should refresh and provide you a login dialog. Use the admin login you just defined to enter. You’re in!

[:image-popup:1_installation_guide/installer/08-finished.png]
