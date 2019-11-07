<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

Binding to an existing LDAP  directory or Microsoft Active Directory (AD)  allows you to reuse the user accounts that you already have in the directory, without having to create new accounts in Pydio. It is therefore  one of the most frequently wanted features  when deploying Pydio on top of an existing infrastructure., even more so if you have e.g. a Windows environment where access to files is managed with AD accounts, and you want to use those file servers as backend storage.

However, correctly setting up the correct parameters can be tricky, as every LDAP deployment is different.

Also note that this is not a Single Sign-on  configuration : your users will still be required  to log in to Pydio, using their LDAP/AD username and password. The Pydio “Admin” account will also still work.



### Requirements
+ you need to have the PHP LDAP module installed, eg  on Ubuntu/Debian : `apt-get install php-ldap`.
+ you can also add phpldapadmin : `apt install phpldapadmin` it will help you administrate your ldap easily.
+ in Pydio you need to  set up a multiple authentication configuration, using the “Secondary Instance” parameter, to allow your existing users to create new ones outside of the directory (e.g. to share files with 3rd parties)
+ in the Authentication Parameters (see previous chapter), you probably want “Auto Create User” set to **_YES_**. Pydio will then create an Pydio user object for each user that logs in with LDAP/AD username and password. You can further use those user object for user rights management in Pydio.
+ in the Authentication Parameters, set  “Transmit Clear Pass” to **_YES_**, at least until your LDAP/AD authentication is set up and working.


### LDAP connexion information
Open the **Application Parameters > Authentication**, and go to the “Master Driver” section. Here you will have to switch to the LDAP driver, and set up the correct values to connect to the server.

This first set of the parameters define how the connexion is done, which username to use as directory administrator to bind, and filters to perform the search query that searches and authenticates  your users.

[:image-popup:2_getting_started/ldap_server_connection_update.png]

##### Description of the parameters :

+ **Server Connection**

    + **URL** : Server hosting the LDAP directory, or a MS AD Domain Controller, … : IP address or FQDN  e.g  192.168.0.14,  dc1.mydomain.tld, localhost, …
    + **Protocol** : You can choose protocols such as SSL(ldaps) or StartTLS.
    + **Port** :  the port your LDAP/AD listens on, usually  389
    + **LDAP bind username and password**. This is optional, and depends on your server configuration. Either you let it empty, and the connexion for searching will be anonymous, or if you set username and password, search will be done via binding.
The user name must be a complete DN, like `cn=admin,dc=domain,dc=ext`.
For authentication agains AD, you usually must provide the username and password of an account in the Domain Admins group. (Best practice is to create a dedicated account, and not use the default Administrator account)


+ **Users Schema**
    + **People DN**  :  The part of your LDAP/AD where Pydio should look for the user accounts. Usually this will be a specific Organizational Unit (OU); typical values  will be for example `ou=People,dc=pydio,dc=org` or `ou=Users,dc=mydomain,dc=tld`.
    this is a repeatable field, i.e you can define it multiple times to allow you to perform the search on multiple branches.
    + **LDAP Filter** : a default filter applied to all search queries, to restrict the search to e.g a certain objectClass, or for example a certain group of users, if your schema is using the membersOf overlay. For AD, try `objectClass=user` or `objectClass=person`
    + **User Attribute** : the LDAP/AD attribute that holds the username with which users will log on.  It will be used in an ldap query together with the LDAP Filter Attribute. E.g. if it is set to “uid”, the final query will search something like `(&(objectClass=person)(uid=userlogin))`.
    For AD, `sAMAccountname` is a good value.

These are all you need for LDAP/AD user authentication. At the bottom of the form, you can see a “Test connection” button : once you have filled the fields mentioned above, you can check if your search is working by entering an existing user login here, and testing the search with the button. You should definitely have it working before saving your configuration and before you continue with additional LDAP/AD configuration.



### Mapping LDAP attributes

[:image-popup:2_getting_started/ldap_attributes_mapping.png]

Once you’ve setup a basic LDAP/AD connection, your LDAP/AD users can log on to Pydio, and an Pydio user object will be created for them when they log on. Essentially, you’ve mapped Pydio logins to LDAP/AD accounts. Next, you will probably want to retrieve additional information out of the LDAP/AD, such as the user’s email address, user’s display name or full name, etc. This is done by mapping additional LDAP/AD attributes to local parameters of Pydio.

These mappings are done login time, and the user data is updated only if a change is detected, to impact low in term of performances.

Mapping of attributes works with a set of parameters  that work in triplet : {ldap attribute, mapping type, plugin parameter}. This set is repeatable, so that you can retrieve and map several distinct LDAP/AD attributes.

+ LDAP attribute : the attribute to retrieve when searching for user, and to get the value from.
+ Mapping type can take 4 values :
    - Plugin parameter : will use the value provided in the following “plugin parameter” field to map the retrieve value to any arbitrary plugin parameter of the current user (see further).
    - Role id : will apply to the current user a role with the value id. Create the role if it does not exists.
    - Group Path : apply the group path to the current user, e.g. /group1/ will create a user being automatically part of group1.
    - Profile : set the specific profile to the current user (to be used with caution).
+ Plugin parameter : if the mapping type is set to plugin parameter, the driver will expect this field to be
    - either a simple text value (like MY_PARAMETER), in which case it will add a parameter with this name linked to the “auth.ldap” plugin (see the Roles filtering section),
    - in the form of PLUGIN_ID/MY_PARAMETER, in which case it will link this parameter to the given plugin id.

An example will probably make this more clear.

##### example

An example of straightforward attribute mapping :

In the `core.conf/manifest.xml` you can find the various parameters provided by the `core.conf plugin`, that actually define the user account data. For instance, you’ll find that Pydio has a `core.conf/email` parameter and a `core.conf/USER_DISPLAY_NAME`, respectively referring to the user “Mail” field and Display name in the GUI.

So, you can map the LDAP “email” attribute or AD “Mail” attribute to the Pydio “core.conf/email” attribute of the authenticated User. Likewise, you can map any suitable LDAP or AD attribute to the Pydio `core.conf/USER_DISPLAY_NAME` parameter to get human-friendly names in the GUI.



##### Using LDAP/AD Groups

When you’re using an LDAP directory that supports the « memberOf » attribute (i.e. you have Microsoft Active Directory, or an LDAP server with the « memberOf » overlay set up), you can use the attribute mapping mechanism to integrate your Active Directory groups in Pydio. This is explained in more detail in the How-To section : [Groups and Roles with LDAP/AD integration](https://pydio.com/en/docs/kb/authentication/advanced-authentication-ldapad)

You will also need a good understanding of Pydio’s Groups and Roles to make good use of this feature.



### Setting up a secondary auth driver
Most generally, LDAP accesses are readonly, thus preventing Pydio users to create new users for sharing. For this reason, you have to configure a secondary instance to keep a parallel user base that is fully manageable by Pydio. Choose either SQL or Serial files, depending on the Configuration storage you already use (SQL or Serial), and depending on the user load you expect. Set the “Mode” in “Master / Slave”, as LDAP will be the master reference. See previous page (Authentication) for more details.


### Advanced Mapping
For a more advanced usage of the LDAP/AD feature you can check our how-to right **[HERE](https://pydio.com/en/docs/kb/authentication/advanced-authentication-ldapad)**.