### Binding to Apache Directory LDAP 
_____________________________________


Historically what has always made Pydio fun is its abilities to re-use and integrate as much as possible resources of environment it is installed in.

Instead of creating manually new users the admin can import and synchronize users from an existing LDAP server and choose how to map LDAP user attributes against Pydio user attributes.


##### LDAP resources are organized as tree of object and a DN or `Distinguished Name` idenitifies an unique object in the tree. Basically DN is similar to path for a file in a file arborescence.


#### LDAP Settings 

In your admin left panel click on "**External Directories**" in the "**IDENTITY MANAGEMENT**" section. And then on the top-right click the "**+ DIRECTORY**" button.


#### **General Options**

|Field| Description|
------|-------------
Label |The name of the directory as it appears in the list of imported directories.
Synchronization| The are specificed the rate and the time the users directory is refreshed.


#### Connection to the server

The LDAP server connection is very straightforward

|Field| Description|
------|-------------
Host |The name of the directory as it appears in the list of imported directories.
Connection Type| Secure or plain.
Binding DN | The object user identifier whose Cells wear roles to laod the users
Binding Password| the binding DN password


In case of a secure connection to a server that uses self-signed certificate some additional parameters can be required.

|Field| Description|
------|-------------
Skip Certificate Verification | Skips the server certificate verification. `Since users data are sensitive it is not recommended to enable this option`. 
Root Certificate Path| The full path of the server certificate
Root Certificate Data| The content of the server certificate
Page size| The number of user displayed by page


#### Filtering user

Filters allow selection of specific user based on they attributes, class, group... and require some some LDAP knowledge to be configured.


|Field| Description|
------|-------------
DN | Define a list of DN Cells will load users from 
Filter| a conjunction of condition in LDAP notation. Here is an example of filter: **(&(objectClass=user)) (memberOf=CN=devs,OU=company,DC=lab))**
Id Attribute Name| The ID of attribute to use as Cells user ID


#### Mapping rule

Most of time, if not always, Cells user attributes and LDAP users attributes have different appelation. Mapping can be defined to make them match.

Mapping has 3 parts:

* The left attribute: the name of the LDAP user object
* The rule string: a string filter for transforming the left attribut
* The right attribute: the name of the Pydio user attribute.



#### the MemberOf Mapping

# TODO