Some other drivers nicely demonstrate how Pydio can be used as a generic data-browsing framework.

### Access.mysql

Disclaimer : If you installed Pydio by the linux repository ( apt / yum ), you must install the "pydio-plugin-access.mysql" package.

First you need to create a workspace with "Mysql Database" storage driver :

[:image-popup:4_setup_workspaces_and_users/create_workspace_mysql.png]

Then you need to add the Host, the database name, the user and the password. After that, as you can see This one will turn your Pydio client into a phpMyAdmin-like interface: you can browse tables and records, and modify the database structure: create/delete tables, create/delete columns from tables:

[:image-popup:4_setup_workspaces_and_users/workspaces_mysql.png]

### Acess.imap

Disclaimer : If you installed Pydio by the linux repository ( apt / yum ), you must install the "pydio-plugin-access.imap" package.

Disclaimer : Don't forget to install the "Mail_mimeDecode" package from pear.

Associated with the Email Viewer (editor.eml), a workspace created with the driver transforms Pydio into a mailbox. View all your mailboxes and the emails, with support for text and HTML, as well as attachment. Only reading is supported, there is no ability to actually Send an email.

This can be handy to transfer a huge attachment directly from an email to another Pydio workspace, without having to download it to your desktop and re-upload it afterward.

A good usecase would probably to create a workspace template and let user add their own email-workspace, providing their own credentials for connexion. Please see the [plugin documentation](https://pydio.com/docs/references/plugins/access/imap/) to get standard configurations for Gmail, Yahoo, etc.

First activate the plugin in "All Plugins" > "Available Plugins" > "Workspaces Drivers" and create a workspace with "Mailbox (Imap or POP)" storage driver:

[:image-popup:4_setup_workspaces_and_users/create_workspace_imap.png]

[:image-popup:4_setup_workspaces_and_users/workspaces_imap.png]

### Access.ajxp_conf / Access.ajxp_shared
These are internal drivers that you should normally not use, but they also demonstrate Pydio flexibility. Ajxp_conf is the driver used for the "Settings" panel.

### Access.jsapi

Disclaimer : If you installed Pydio by the linux repository ( apt / yum ), you must install the "pydio-plugin-access.jsapi" package.

Finally, the "Javascript API" driver is an interesting implementation of a client-side only data provider for Pydio. When switching to a workspace based on this driver, the interface will list all defined Classes and Interfaces defined in the Javascript framework: this is done by introspecting the classes, without speaking to the server, except for displaying the source code.

This could be a good inspiration for creating a serverless (or unhosted) version of Pydio that could grab its data from other data source than from the server itself.
