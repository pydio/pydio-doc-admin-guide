Pydio Cells provides advanced features for managing your users directory. In a simple setup, using the internal directory should be sufficient for most cases. 

Using the SSO features, you can also bind to existing directories, allowing your in-house collaborators to share files with external users. This section will go through the management the internal directory.

Going to `Cells Console > People` section, you can easily create new users and organize them by groups. Groups allow you to organize users in folders like you would with files. In a similar way, one user can only belong to one group. 

## Users

To create a user go, click on **+USER** located on the top right. The following menu will appear:

[:image:3_identity_management/create_user.png]

- **User Login**: this can **not** be changed afterwards.  
- **User Password**: Put the password of the user, you will have to type it twice to ensure it's validity. Of course, make sure to always use strong passwords. Cells Enterprise provides a customizable complexity plugin for passwords.

Once created, editing a user will provide you full control about a user information : 

- **User Profile** : this global category provides high-level set of permissions to the user. It can be used in cunjonction with roles. Possible values are :
   * _standard_ : the most common profile 
   * _shared_ : a restricted profile applied automatically to external users created by standard users
   * _admin_ : a super user with access to the Cells Console and most of the internal resources.
- **Name** : a human-friendly label for this user (editable by the user)
- **Email** : email adress to receive notifications and invitations (editable by the user)
- **Language** : default language for showing the interface (editable by the user)
- **Maximum Shared Users** : an optional limit for the number of shared users this user is allowed to create.

This screens also gives you access to useful actions : 

[:image:3_identity_management/users-actions.png]

- **Change Password** : manually reset the user password
- **Lock out / Reactivate** : instantly disconnect the user from all clients, or reactivate its access if it has been automatically locked out after too many failed login attemps. This `unlock` operation can also be done using the command line.
- **Force Password Change** : if switched on, at the next connection, the user will be presented with a password change dialog and cannot login until she updates her password.

The other Tabs of the user editor are used to manage the users permissions (see the Access Controls chapter to learn more). Finally, the profile can be used to filter the users list, and _shared_ and _admin_ users are highlighted by a special icon in this list.

## Groups

Groups allow you to manage the rights, parameters and actions of many users at once. It is usually used to regroup users that have the same usage of Pydio Cells, access the same workspaces, etc.

To create a group, click on **+GROUP** located on the top right. The following menu appears:

[:image:3_identity_management/create_group.png]

- **Group Identifier**: how the groups is identified in pydio's file system (useful for api etc...).
- **Group Label**: human friendly name for this group.

If you need to move a user from one group to another, simply drag'n'drop this user in the desired group in the tree.


## Additional operations

The search box at the top of the `People` screen allows you to quickly find users by their login or name. Once in search result mode, you can edit or delete a user, or move it to a group as required.

The BULK ACTIONS button allows you to select all users in page to delete them at once. When selecting the whole page, only users are selected, not groups.

--------------
_See Also_

 - [Using SSO features to bind to external directories](./single-sign-features)
 - [Managing permissions on Users, Groups and Roles](./users-roles-and-groups)