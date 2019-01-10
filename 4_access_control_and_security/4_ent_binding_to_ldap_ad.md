_Ldap authentication plugin is the most important plugin in Cells Enterprise. It enables Cells to be well integrated with existing system such as a SSO solution._

In Cells, a ldap server will be seen as an external authentication source. With flexible parameters and options in configuration, you can quickly extract from your directory a list of user to be imported to Cells. The configuration to map users as similar as the one in Pydio PHP( Pydio 8). An advanced feature of ldap module in Cells now enables you to add many ldap domains as many as you want.

## Configuration

### Step 1

Login with admin user and go to **Settings**. Click on **External Directory**.
To add new directory, click on "DIRECTORY" or click on existing ldap to modify it's configuration.

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_862.png]

### Step 2

In this step, you will create a ldap directory to connect an openldap server.

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_863.png]

#### Basic settings for connection to ldap Server

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_873.png]

##### 1) IP address of host name of ldap server and Port

Example:

- 127.0.0.1:389
- ldap.domain.com:389
- ldap.domain.com:639

If you do not specify the port in the server address, the default port will be 389.

##### 2) Connection Type

##### 3) Binding DN: Depending on ldap, this field accept user id, distinguished Name, or username@domain.com format

For example:

- pydio@lab.py
- cn=pydio,ou=company,dc=lab,dc=py

##### Active Directory Notes

If you are working with an Active Directory, you can get the dN of user object

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_874.png]

> Note: It's highly recommended to use an ldap user and delegate this user to be allowed to do "*Read all user information*" task. Please visit this link for further information: https://www.msptechs.com/safely-delegate-control-active-directory/

##### 4) User's password

##### Advanced Settings

##### 5) Skip certificate verification

If this option is turned on, pydio will verify the certificate of the ldap server before connecting. Please make sure that the CA certificate is trusted by the system (OS).

##### 6 & 7) SSL or STARTTLS connection

We need to specify to Pydio the absolute path to the certificate of the ldap server (6) or the content of this certificate in base64 format (7)

> Note: You can easily get a certificate by using the following command:

```bash
 openssl s_client -showcerts -connect ldapserver.com:389 </dev/null 2>/dev/null|openssl x509 -outform PEM >mycertfile.pem
```

##### 8) Default paging size

500 records is the default value in openldap, and 1000 is the number of records for one page in the Active Directory.

#### User Filter

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_875.png]

##### 1) User's DN

You can define more than one distinguished name (DN) of an organization unit (or a container in Active Directory) in the ldap tree, Pydio will search for the users to import to its database.

For example:

- ou=company,dc=vpydio,dc=fr
- ou=visitor,dc=vpydio,dc=fr

##### 2) Filter

If you are working for the first time with ldap, please visit [https://social.technet.microsoft.com/wiki/contents/articles/5392.active-directory-ldap-syntax-filters.aspx](https://social.technet.microsoft.com/wiki/contents/articles/5392.active-directory-ldap-syntax-filters.aspx) for more information about ldap syntax and examples.

Filter specifies the conditions that must be met for users in ldap to be imported to Pydio. In this filter we usually combine a clause to filter only the user object class with a condition.

*Example:*

(objectClass=user)

(objectClass=person)

(objectClass=inetOrgPersion)

*Example:* Filter user objects in ldap whose *department* attribute value is *staff*

```conf
(&(objectClass=user)(department=staff))
```

**Note:**
It's highly recommended to create in the Active Directory a specific security group for Pydio to make the filtering string more simple.

*Example:* In Active Directory we create a security group in organization *company* and add users, groups to this security group. 
In pydio the filter will look like this :

```conf
 (&(objectClass=user)(memberOf=CN=staff,OU=company,DC=lab,DC=py))
```

**Note:**
In the Active Directory if you want to search users in a nested group in the Active Directory, the memberOf attribute should be `memberOf:1.2.840.113556.1.4.1941`

*Example:*

  `(&(objectClass=user)(memberOf:1.2.840.113556.1.4.1941:=CN=staff,OU=company,DC=lab,DC=py))`

This filter will get all members of the staff group including all members in the nested groups.

#### Mapping user's attribute

You can map user's attributes in ldap to pydio by defining some mapping rules.

There are three parts in each rule:

- Left Attribute: is the attribute name of user object in ldapserver
- Right Attribute: is the attribute name of the user object in pydio. _They are case sensitive names e.g: **displayName**, **email**, **Roles**_
- Rule String: You can define this string as a filter for the mapping process. It can be blank, contain a list of value, or a regular expression string.

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_867.png]

Example: *department* is an attribute of user object in ldap, it accepts the following values: _finance, admin, hr, marketing, it_helpdesk, it_hardware_.
But you would like to map only **"admin"**, **"it_helpdesk"**, **"it_hardware"**  values to Roles in Pydio to do so, define them as the following:

    Left Attribute: department
    Rule String: admin,it_helpdesk,it_hardware
    Right Attribute: Roles

If you would like to map only values starting by **"it_"**, in this case, you can use preg format.

    Left Attribute: department
    Rule String: preg:^it_
    Right Attribute: Roles

#### MemberOf mapping

1) MemberOf mapping is a specific case of mapping user's attribute to Roles. MemberOf is a multiple value attribute of user object which itself contains a list of groups where this user is a member. You should define a rule.

```conf
  Left Attribute: memberOf
  Rule String:
  Right Attribute: Roles
```

The values of *memberOf* attribute can contain any group in the ldap directory. Thats why you need to define group filtering to precise the list of groups to be mapped with Pydio.

2) Group DN: is the DN of one or more organization unit in the ldap directory where pydio will search for groups. If memberOf values have some groups in other locations, they will be ignored.

3) Group Filter: Like **User Filter**

Example: `(objectClass=group)` or `(objectClass=groupOfNames)`

4) Id Attribute: Pydio will take the value of this attribute of group object to use as **Role id** in Pydio.

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_876.png]

Some ldap directories does not support *memberOf* attribute by default, if you turn off "Native MemberOf support", Pydio will try to calculate this attribute from "Fake memberof Attribute" and its format.

- Fake memberof Attribute: is the name of the attribute of group object which holds the member identity. In opendlap, this value is usually **member** or **memberuid**
- Depending on the value of 'Fake memberof Attribute' and the schema of ldap, the format is usually 'dN' or 'uid'

Values of two options are usually:

```conf
  Fake memberOf Attribute: member
  Fake memberOf Attribute Format: dn
```

or 

```conf
  Fake memberOf Attribute: memberuid
  Fake memberOf Attribute Format: uid
```

[:image-popup:4_access_control_and_security/2_13_ldap/Selection_877.png]