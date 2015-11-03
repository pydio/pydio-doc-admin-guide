### Data concerned & available plugins
As explained in the previous chapter, there are various types of data managed in AjaXplorer : configurations vs. “business” data ( = your files ). All the configurations data are managed by the configuration backend (humm, you would have guessed), but the users directory is handled by another type of plugins, Authentication ones. Basically, these plugins are just able to tell whether a user exists or not, if he can be logged in with a given password, and if he is part of group.

Although configuration backends are few (basically serialized files or DB), authentication backend have more implementations :

+ auth.serial, auth.sql : “Internal“ implementations, that keep all data inside AjaXplorer, and that are very close to their “conf” counter part.
+ auth.ldap, auth.cas : SSO like implementations that can query an LDAP/AD directory or a CAS server.
+ auth.ftp, auth.smb : specific implementations that are linked to a workspace for performing authentication “dynamically”.
+ auth.remote, auth.cmsms, auth.phpbb : bridges allowing to use an external PHP CMS as a user directory. Currently there are WordPress, Joomla and Drupal counterparts, as well as detailed how-to to log to any existing system.

The following articles will go through the most used implementations. Use the Global Configurations > Core Configs > Authentification panel to switch the backend.

### One ore two instances? Master or Slave?
If you open the Authentification panel, below the first set of generic parameters, you will see two sections : Main Instance & Secondary Instance (optional).

[:image-popup:global_parameters/setup_authentication_driver/screenshot-2013-05-08-at-14-55-45.png]

By default, depending on the value you chose at install, you will see the “Main Instance” set with a value (either Serialized or DB) and the associated options, and the secondary instance is not used.

This secondary instance can be used in two different situations :

+ **You want to “merge” two different data sources** for authenticating the users. For example, your company already manages two directories, one for internal users and the other for external collaborators, and you want to keep them separate. You also want to **let the user choose at login time** to which backend she will log. You use the “User Choice” Mode and define a secondary auth plugin instance for that.
+ **Your primary directory is readonly**. It is for example very frequently the case if you are connecting to a central LDAP directory. In that case, if you define your Main Instance to point to this directory, users of AjaXplorer will not be able to create new temporary users to share folders! That’s why you will configure a secondary instance using one of the ajaxplorer “internal” implementation (serialized files or DB), which will maintain a secondary directory for external users, without having to pollute your central directory. Using the **“Master / Slave” mode**, will put this second instance as a slave of the first one, allowing also better performances and caching the original users inside the secondary store as well.
Note : for those familiary with the previous versions of AjaXplorer, this is nothing more than the encapsulation of the **auth.multi** plugin.

### Auth plugins common options
As you can see when you select the various possible AUTH plugins, some options are commons to all of them. Some parameters worth to be noticed : 

**Transmit Clear Pass**: if set to NO, this setting enables a simple hashing of the password directly in the clients, avoiding a clear pass transmission on the network. However , the drawback of this is that it will break many features requiring the transmission of the password from AjaXplorer to a third party tier (like the webdav server http authentication, or when connecting to a remote server, etc…) . Thus, it should be preferred simply to an HTTPS connexion that would in any case transmit an encrypted password.

**Store Password in Session** : this option can be necessary in the case of a “dynamic” authentification to a remote system, for example for mounting a folder using the current user credentials. 

**Auto Create User** : Generally, this should be set to true if you are binding AjaXplorer to an external users directory. This is highlighting the separation between the proper authentication data and the configurations data : when a user will log in for the very first time, if he is correctly authenticated by the “Auth” plugin, the whole Ajaxplorer counterpart storing his/her preferences, rights, etc, does not exists by default. If this option is set to Yes, and AjaXplorer user object will be created, with the various defaults values applied (default profile and role, default rights from the workspaces, etc…).

[:summary]