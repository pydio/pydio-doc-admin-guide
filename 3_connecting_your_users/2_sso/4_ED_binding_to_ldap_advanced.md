One of the most powerful feature of LDAP Binding is dynamic mapping ldap groups to roles in Pydio. With this feature, the administrator focus his attention only on the management of users, groups in ldap server and get rid of coping the setting to pydio Cells. This article describes the underlying idea as well as the best practice of configuration of mapping.

If your LDAP server supports the `memberof-overlay` in ldap filters, each user object in LDAP response may have "memberOf" attribute. The list of group may contain system groups that we don't want to import to. In general, we are looking for only a small set of group. We provide a configuration interface with some simple parameters, but very flexible that help you refine the groups to be imported to Cells.

* Microsoft Activie Directory supports natively `mamberof-overlay`
* If your ldap does not support `memberof-overlay`, we provide you magic parameters to force your ldap to list off the `memberof` value that, in turn, provides the input for the mapping.
  
## MemberOf mapping

This is a specific case of mapping, in order for a user to be assigned one or more roles in Pydio Cells depending on the `memberOf` user attribute that is defined in your external directory. `memberOf` is a multiple value attribute of user object which itself contains a list of groups where this user is a member. You should define a rule.

- **Left Attribute:** `memberOf`
- **Rule String:**
- **Right Attribute:** `Roles`

The values of `memberOf` attribute can contain any group of the LDAP directory: you might want to narrow down the list of the groups that are to be mapped in Cells by using group filtering.

[:image-popup:3_connecting_your_users/ldap/ldap_4.png]

_The above screenshot shows an example of `memberOf` mapping, where:_

1) toggles this feature ON and OFF

2) Mapping: defines the name of the attribute that is to be used *in your LDAP server*. This is useful to emulate the `memberOf` feature if it is not supported by your implementation.

3) DN: is the DN of one or more organization unit in the LDAP directory in which the connector searches for groups. If memberOf values have some groups in other locations, they will be ignored.

4) Group Filter: Like **User Filter**
  Example: `(objectClass=group)` or `(objectClass=groupOfNames)`

5) ID Attribute: Pydio will take the value of this attribute of group object to use as **Role ID** in Cells.

Some LDAP directories do not support `memberOf` attribute by default.  
If you turn off the `Native MemberOf support` toggle, Cells tries to calculate this attribute from the `Fake memberof Attribute` value and its format.

- Fake `memberOf` Attribute: name of the attribute of group object which holds the member identity. In OpendLDAP, this value is usually `member` or `memberuid`.
- Depending on the value of `Fake memberOf Attribute` and the schema of your LDAP, the corresponding format is usually `dn` or `uid`.

Values of these two options are usually:

- **Fake memberOf Attribute:** `member`
- **Fake memberOf Attribute Format:** `dn`

or

- **Fake memberOf Attribute:** `memberuid`
- **Fake memberOf Attribute Format:** `uid`

## Policy fine-tuning

As explained in the introduction, and due to the Master/Slave model of the integration, we strongly advise you to perform following fine tuning of your instance.

### Remove My Account button

The easiest way to prevent LDAP-defined users to change information that are defined in your external directory(ies), is to define a system wide rule that will disable the `My Account` button for the relevant users. This is achieved easily:

- As an admin user, go to: `Cells Console >> Identity management >> People`,
- Open for edit the parent group of your LDAP users,
- Open the `Application Parameters` page,
- In the upper right corner, choose the `All Workspaces` option from the `Add for...` drop down list,
- Search for the `My Account` action of the `action.user` category by simply typing `my account` in the quick search field, and select it,
- Ensure the MyAccount toggle is turned off.

You are then set: the `My Account` button is then hidden for all corresponding users.  
Depending on your specific configuration, you might want to do this for various groups or define a specific role that can be applied to relevant groups and users.