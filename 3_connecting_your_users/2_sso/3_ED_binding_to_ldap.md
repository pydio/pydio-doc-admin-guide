In Cells, a LDAP server is seen as an external authentication source. You can extract a subset of users from your directory to be imported in Cells, using flexible parameters and configuration options. Cells can be bound to more than one directory server with as many LDAP domains as you want.

## Overview

Generally speaking, the integration is achieved using a Master/Slave model, where your LDAP is the master. Any modification done in your LDAP will overwrite the corresponding value in Cells during the next synchronization.

As a consequence, we strongly advise that you configure your user interface to prevent the end-user from manually modifying any attribute that is mapped to an existing LDAP attribute.  
Typically you should remove the `My Account` button from the various menu for LDAP defined users.  
See last paragraph of this section for more info on this.

The synchronization happens regularly (time and frequency are defined for each connection) and is managed by the scheduler: the configuration seen below only creates a new job in the scheduler.  
It is then possible to get more info about the synchronisation status and to trigger a resync manually:

- Go to `Cells Console >> Backend >> Scheduler`
- Open editor for the synchronisation task by clicking on it in the list.  
  It is usually named: `<name of your LDAP connexion> > Synchronize external directories`

### Supported Servers

The LDAP connector embedded in Cells ED is compliant with the LDAP standard. Thus, it can be used with servers that are implementing this protocol.  
You must install and configure your server before connecting it in Pydio Cells.  
Internally, we use the 2 below servers for development and testing purposes. They are known to integrate smoothly with Cells:

- [OpenLDAP](https://openldap.org): a free, open source implementation of the Lightweight Directory Access Protocol developed by the OpenLDAP Project
- [Active Directory](https://en.wikipedia.org/wiki/Active_Directory): a directory service of Microsoft that supports LDAP versions 2 and 3

Fell free to contact us if you have questions about another specific implementation.

### Create or edit a connection to an existing LDAP server

Log in with an admin user and go to:  
`Cells Console >> Identity Management >> External Directory`

Then, click on:

- `+ DIRECTORY` to register a new directory,
- an existing LDAP configuration to modify it.

[:image-popup:3_connecting_your_users/ldap/ldap_0.png]

## Configuration

### General Option

[:image-popup:3_connecting_your_users/ldap/ldap_1.png]

Fields are here explicit. It is usually good practice to name this connection using the corresponding domain name.

### Server Connection

[:image-popup:3_connecting_your_users/ldap/ldap_2.png]

_Use the numbers inside the above screenshot to refer to the settings_.

#### 1) Host

The IP address (or hostname) and port of your server. For instance:

```conf
127.0.0.1:389
ldap.domain.com:389
ldap.domain.com:639
```

If you do not specify any port, default port `389` is used.

#### 2) Connection Type

With this option, you define the type of connection you use. It is not recommended to use an unsecured connection if you are not in a secured private LAN.

#### 3) Binding DN

The distinguished name of a power user in your directory that has sufficient privileges to list other users and groups.  
This field accepts user ID, distinguished name or email. Choose the format that is relevant, depending on the LDAP software you use. For instance:

```conf
pydio-admin@example.com
cn=pydio-admin,ou=company,dc=example,dc=com
```

**Note:** in Active Directory, it is highly recommended to use a dedicated LDAP user with delegated read privileges on the corresponding users. This can be done via the `Read all user information` task. You can visit [this link for further information](https://stealthbits.com/blog/delegated-permissions-in-active-directory/).  
In Active Directory, you can get the dn of the chosen user user via the properties popup:

[:image-popup:3_connecting_your_users/ldap/Selection_874.png]

#### 4) Binding Password

The password of the above user

#### 5) Skip certificate verification

Turn this option ON if you are using a self-signed certificate **that you trust**. Use this option with extra care and at your own risk.

#### 6) Root Certificate Path

This is only used for TLS and StartTLS connections.
You need to specify the absolute path to the certificate of the LDAP server

#### 7) Root Certificate Data

This option allow you directly use a base64 format certificate to config the secured ldap connection. This is a base 64 non-linebreak string.

**Note:** You can retrieve the public part of your server's certificate using the following command. This command also allow you test the 

```sh
openssl s_client -showcerts -connect ldapserver.com:389 </dev/null 2>/dev/null|openssl x509 -outform PEM >mycertfile.pem
```

#### 8) Page size

This defines the maximum number of objects that are returned at each call.

Please note that the maximum page size is defined on the server (hard limit), but you might use this option to reduce page size on the client size.
For the record, default page size is 500 in OpenLDAP and 1000 in Active Directory.

### User Filter

You have to define **at least one** user filter to define which users are to be imported from your external directory to Pydio Cells. You might only import a subset of your existing user base, depending on the filter you define.  
The tab looks like this:

[:image-popup:3_connecting_your_users/ldap/ldap_3.png]

_Use the numbers inside the above screenshot to refer to the settings_.

#### 1) DN

You can define more than one distinguished name (dn) of an organization unit (or a container in Active Directory) in the LDAP tree.  
Cells will retrieve the corresponding users and import them to its internal user directory.  
For instance:

```conf
ou=company,dc=example,dc=com
ou=visitor,dc=example,dc=com
```

#### 2) Filter

Additionally, you have to specify at least on simple condition that must be met for a LDAP user to be imported in Cells. For instance:

- Filtering by object class:
  - `(objectClass=user)`
  - `(objectClass=person)`
  - `(objectClass=inetOrgPerson)`
- Only import users whose *department* attribute value is *staff*:
  - `(&(objectClass=user)(department=staff))`

If you are not very familiar with LDAP filter, you might find [this article on Microsoft website](https://social.technet.microsoft.com/wiki/contents/articles/5392.active-directory-ldap-syntax-filters.aspx) useful to get more information about LDAP syntax and get further examples.

When using Active Directory, it is highly recommended to create a specific security group to ease filtering. Typically creating a `staff` security group in the `company` organisation and adding users and groups to it.

In Cells, the filter will look like this :

```conf
 (&(objectClass=user)(memberOf=CN=staff,OU=company,DC=example,DC=org))
```

In Active Directory, if you want to also retrieve users from nested groups, you must specify this `memberOf` attribute: `memberOf:1.2.840.113556.1.4.1941`.  
For instance, this retrieves all members of the `staff` group including all members in the nested groups.

```conf
(&(objectClass=user)(memberOf:1.2.840.113556.1.4.1941:=CN=staff,OU=company,DC=example,DC=com))
```

### Simple Mappings

You can map user attributes that are defined in your LDAP to user attributes in Pydio Cells. Each mapping is defined by a rule. A rule has three parts:

- **Left Attribute**: the attribute name of user object in the LDAP server
- **Right Attribute**: the attribute name of the user object in Cells.
- **Rule String**: you might add a filter for the mapping process. This field can be blank, contain a list of value or a regular expression.

_**Warning**: In Pydio Cells, user attribute names are **case sensitive**_.  
_E.g: displayName, email, Roles_

#### A basic example

Let us say you have a `department` attribute on user objects in your LDAP model.  
This attribute can have following values:

- `finance`
- `admin`
- `hr`
- `marketing`
- `it_helpdesk`
- `it_hardware`

But you only want to map `admin`, `it_helpdesk` and `it_hardware` values to Roles in Pydio Cells.  
In order to do so, you might define the following mapping rule:

- **Left Attribute**: `department`
- **Rule String**: `admin,it_helpdesk,it_hardware`
- **Right Attribute**: `Roles`

If you need to only map values starting by `it_`, you can use the _preg_ format.

- **Left Attribute:** `department`
- **Rule String:** `preg:^it_`
- **Right Attribute:** `Roles`


