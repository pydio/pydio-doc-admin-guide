This is a recommanded setting if you

+ Don’t have to connect to an existing users directory
+ Know that you will have quite a lot of users (more than 100) and a high load on the system.
+ Have a mysql server running and ready to use on some server

The simplest method is to switch to Database storage at the very first AjaXplorer installation. If it was the case, then you have nothing to do.

If you have e.g. selected a non-db setup at startup to make some quick tests, and now want to switch to a more robust setup, follow the steps :

+ Gather the necessary data for the SQL connexion : MySQL server Host, Database (must be already created), username and password. User must have all privileges on the database.
+ Go through the step indicated in the SQL section of the following page [Setting up configuration storage](https://pyd.io/administrator/configuring-global-parameters/setting-up-configuration-storage/), included the “RECOMMANDED” step : once the Core Connexion is defined in the Configurations Management panel, save and close, open the Authentication panel, switch the main instance to SQL and click on “Install SQL Tables” button. Then Save and go back to the rest of the procedure for the configurations.

One you are logged out, if the database is empty, an “admin” user will be automatically created to let you log in. You should right now change this user’s password.