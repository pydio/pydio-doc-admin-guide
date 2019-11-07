<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

### Configurations backends
There are two major data types handle by Pydio: your actual “business” data, ie. your files and their metadata, and “configurations” data, i.e. plugins options and other internal parameters. Configurations are themselves divided in two parts: users credentials, managed by AUTH plugins, and all the rest, managed by CONF plugins. We are only speaking here of the latter sort.

### Data concerned
+ users preferences, rights, roles and profiles.
+ workspaces definitions (previously “repositories”)
+ plugins configurations
+ other binaries and data registered by other plugins. E.g. notifications.

Currently, there are two ways to store the configurations data: either in serialized files stored on filesystem (under the AJXP_DATA_PATH , data/ by default, defined in conf/bootstrap_context.php), or inside an SQL database (see below).

### Enabling SQL storage
If you have already configured a database storage when installing the application, you probably don’t have to go through this.

Currently, MySQL and Sqlite are supported. For both, you will need to make sure that the corresponding php extension is installed. Configuring the conf.sql plugin will be a multiple step action:

+ Inside the Application Parameters > Configs Backends, first define an SQL Connexion: choose the correct driver (MySQL or Sqlite) and fill the options values accordingly.
    - For MySQL
        * Host: generally localhost, or a server name provided by your hosting
        * Database: an existing database name
        * User: a MySQL user with all privileges on this database
        * Password: the mysql user password.
    - For Sqlite
        * A filename on the server. Make sure it points to a folder that is writeable by your webserver, and that is NOT web-accessible.
    - Save the options.
+ Still in the same panel (Configurations Management), now scroll down to “Instance Param” section and switch the to the DB Storage value. Leave the connexion value to “Core Connexion” by default, and click on “Install SQL Tables” button. If there are connexion problems, you may see red errors appear. Otherwise, green message will appear. The SQL instruction contain CREATE TABLE IF NOT EXISTS, so you will not override existing tables.
+ Save: you will be automatically logged out, as switching configuration backend is quite fundamental in the lifecycle of the application.
