Ldap authentication plugin is the most important plugins in Cells Enterprise. It makes Cells to be well integrated to existing system as a SSO solution. 

In Cells, a ldap server will be seen as a external authentication source. With flexibles parameters and options in configuration, you can extract quickly from your directory a list of user to be imported to Cells. The configuration of mapping user is similar with config in Pydio PHP. An advanced feature of ldap module in Cells is you are now able to add many ldap domain as you want. 

## Configuration:

### Step 1:
Login with admin user and go to "Settings". Click on External Directory.
To add new directory, click on "DIRECTORY" or click on existed ldap to modify configurations.

<img src="http://images.vpydio.fr/local/Selection_862.png" alt="" width="600">

### Step 2:
In this step, you will create a ldap directory to connect to an openldap server.

<img src="http://images.vpydio.fr/local/Selection_863.png" alt="" width="600">

#### Basic settings for connection to ldap Server

<img src="http://images.vpydio.fr/local/Selection_873.png" alt="" width="600">

##### 1) IP address of host name of ldap server and Port

Example:

- 127.0.0.1:389
- ldap.domain.com:389
- ldap.domain.com:639

If you do not specify the port in sever address, the default port is 389

##### 2) Connection Type:

##### 3) Binding DN: Dependings on ldap, this field accept user id, distinguished Name, or username@domain.com format
For example:
- pydio@lab.py
- cn=pydio,ou=company,dc=lab,dc=py

##### Active Directory Notes

If you are working with Active Directory, you can get dN of user object

<img src="http://images.vpydio.fr/local/Selection_874.png" alt="" width="375">

> Note: It's highly recommended to use an ldap user and delegate this user to allow to do "*Read all user information*" task. Please visit this link for further information: https://www.msptechs.com/safely-delegate-control-active-directory/

##### 4) User's password

##### Advanced Settings

##### 5) Skip certificate verification:
If this option is turn on, pydio will verify the certificate of ldap server before start to connect. Please make sure that the CA certificate is trusted by system (OS).

##### 6) 7) SSL or STARTTLS connection:
We need to specify Pydio the absoluted path to the certificate of ldap server (6) or the content of this certificate in base64 format (7)

> Note: You can easily get certificate by using following command

> `openssl s_client -showcerts -connect ldapserver.com:389 </dev/null 2>/dev/null|openssl x509 -outform PEM >mycertfile.pem `

##### 8) Default paging size:
500 records is default value in openldap, and 1000 is number of record for one page in Active Directory

#### User Filter
<img src="http://images.vpydio.fr/local/Selection_875.png" alt="" width="600">

##### 1) User's DN:
You can define more than one distinguished name (DN) of an organization unit (or a container in Active Directory) in ldap tree where Pydio will looking for users to import to its database.

For example:

- ou=company,dc=vpydio,dc=fr
- ou=visitor,dc=vpydio,dc=fr

##### 2) Filter

If you are working at first time with ldap, please visit https://social.technet.microsoft.com/wiki/contents/articles/5392.active-directory-ldap-syntax-filters.aspx for more information about ldap syntax and examples.

Filter specifies the conditions that must be met for users in ldap to be imported to Pydio. In this filter we usually combine a clause to filter only user object class with another conditions.

*Example:*

(objectClass=user)

(objectClass=person)

(objectClass=inetOrgPersion)

*Example:* Filter user objects in ldap whose *department* attribute value is *staff*

  `(&(objectClass=user)(department=staff))`

**Note:**
It's highly recommended to create in Active Directory a specific security group for Pydio to make filter string more simple.

*Example:* In Active Directory we create a security group in organization *company* and add users, groups to this group. In pydio the filter can be :

  `(&(objectClass=user)(memberOf=CN=staff,OU=company,DC=lab,DC=py))`

**Note:**
In Active Diretory if you would like to search users in nested group in Active Directory, the memberOf attribute should be memberOf:1.2.840.113556.1.4.1941:

*Example:*

  `(&(objectClass=user)(memberOf:1.2.840.113556.1.4.1941:=CN=staff,OU=company,DC=lab,DC=py))`

This filter will get all member of staff group included all member in nested groups.

#### Mapping user's attribute

You can map user's attributes in ldap to pydio by defining some mapping rules.
There are three parts in each rule:
- Left Attribute: is attribute name of user object in ldapserver
- Right Attribute: is attribute name of user object in pydio. They are casesensitive names e.g: **displayName**, **email**, **Roles**
- Rule String: You can define this string as a filter for mapping process. It can be blank, a list of value, or regular expression string.


<img src="http://images.vpydio.fr/local/Selection_867.png" alt="" width="600">

Example: *department* is an attribute of user object in ldap, it accepts following values: finance, admin, hr, marketing, it_helpdesk, it_hardware. But you would like to map only "admin", "it_helpdesk", "it_hardware"  values to Roles in Pydio, you can define as following:

    Left Attribute: department
    Rule String: admin,it_helpdesk,it_hardware
    Right Attribute: Roles

If you would like to map only values starting by "it_", in this case, you can use preg format.

    Left Attribute: department
    Rule String: preg:^it_
    Right Attribute: Roles



#### MemberOf mapping

2) MemberOf mapping is a specific case of mapping user's attribute to Roles. MemberOf is a multiple value attribute of user object which contents a list of group where this user is a member. You should define a rule

```
  Left Attribute: memberOf
  Rule String:
  Right Attribute: Roles
```

The values of *memberOf* attribute can content any group in ldap directory. That why you need to define group filtering to precise a list of group to be mapped to Pydio.


3) Group DN: is the DN of one ore more organization unit in ldap directory where pydio will look for groups. If memberOf values has some groups in other locations, they will be ignored.

4) Group Filter: Like **User Filter**

Example: `(objectClass=group)` or `(objectClass=groupOfNames)`

5) Id Attribute: Pydio will take the value of this attribute of group object to use as Role id in Pydio.

<img src="http://images.vpydio.fr/local/Selection_876.png" alt="" width="600">

Some ldap directories does not support *memberOf* attribute by default, if you turn off "Native MemberOf support", Pydio will try to calculate this attribute from "Fake memberof Attribute" and its format.

- Fake memberof Attribute: is name of attribute of group object which hole member identity. In opendlap, this value is usually **member** or **memberuid**
- Depending on value of 'Fake memberof Attribute' and schema of ldap, the format is usually 'dN' or 'uid'

Values of two options are usually:

```
  Fake memberOf Attribute: member
  Fake memberOf Attribute Format: dn
```
or 

```
  Fake memberOf Attribute: memberuid
  Fake memberOf Attribute Format: uid
```


<img src="http://images.vpydio.fr/local/Selection_877.png" alt="" width="600">