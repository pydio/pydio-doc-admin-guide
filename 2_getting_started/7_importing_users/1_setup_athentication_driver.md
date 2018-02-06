### Data concerned & available plugins
As explained in the previous chapter, there are various types of data managed in Pydio: configurations vs. “business” data ( = your files ). All the configurations data are managed by the configuration backend, but the users directory is handled by another type of plugins, Authentication ones. Basically, these plugins are just able to tell whether a user exists or not, if he can be logged in with a given password, and if he is part of group.

Although configuration backends are few (basically serialized files or DB), authentication backend have more implementations:

+ auth.serial, auth.sql: “Internal“ implementations, that keep all data inside Pydio, and that are very close to their “conf” counter part.
+ auth.ldap, auth.cas: SSO like implementations that can query an LDAP/AD directory or a CAS server.
+ auth.ftp, auth.smb: specific implementations that are linked to a workspace for performing authentication “dynamically”.
+ auth.remote, auth.cmsms, auth.phpbb: bridges allowing to use an external PHP CMS as a user directory. Currently there are WordPress, Joomla and Drupal counterparts, as well as detailed **[how-to](https://pydio.com/en/docs/kb/authentication/authentication-your-cms)** to log to any existing system.

The following articles will go through the most used implementations. Use the **Application Parameters > Authentication** to switch the backend.

### One ore two instances? Master or Slave?
If you open the Authentication panel, below the first set of generic parameters, you will see two sections: Main Instance & Secondary Instance (optional).

[:image-popup:2_getting_started/setup_authentication_driver_update.png]

For Master Driver :

[:image-popup:2_getting_started/setup_authentication_master_driver.png]

By default, depending on the value you chose at install, you will see the “Main Instance” set with a value (either Serialized or DB) and the associated options, and the secondary instance is not used.

This secondary instance can be used in two different situations:

+ **You want to “merge” two different data sources** for authenticating the users. For example, your company already manages two directories, one for internal users and the other for external collaborators, and you want to keep them separate. You also want to **let the user choose at login time** to which backend she will log. You use the “User Choice” Mode and define a secondary auth plugin instance for that.
+ **Your primary directory is readonly**. It is for example very frequently the case if you are connecting to a central LDAP directory. In that case, if you define your Main Instance to point to this directory, users of Pydio will not be able to create new temporary users to share folders! That’s why you will configure a secondary instance using one of the Pydio “internal” implementation (serialized files or DB), which will maintain a secondary directory for external users, without having to pollute your central directory. Using **the “Master / Slave” mode**, will put this second instance as a slave of the first one, allowing also better performances and caching the original users inside the secondary store as well.

>If you're going to use a remote authentification you should in most cases put the Pydio database in the secondary driver so that if you have an issue with you're remote authentifcation you can log with your Pydio database and administrate.

Note: for those familiar with the previous versions of Pydio, this is nothing more than the encapsulation of the **auth.multi** plugin.

### Auth plugins common options
As you can see when you select the various possible AUTH plugins, some options are commons to all of them. Some parameters worth to be noticed:

**Transmit Clear Pass**: This is deprecated, leave this value to Yes.

**Store Password in Session**: this option can be necessary in the case of a “dynamic” authentication to a remote system, for example for mounting a folder using the current user credentials.

**Auto Create User**: Generally, this should be set to true if you are binding Pydio to an external users directory. This is highlighting the separation between the proper authentication data and the configurations data: when a user will log in for the very first time, if he is correctly authenticated by the “Auth” plugin, the whole Pydio counterpart storing his/her preferences, rights, etc, does not exists by default. If this option is set to Yes, and Pydio user object will be created, with the various defaults values applied (default profile and role, default rights from the workspaces, etc…).
