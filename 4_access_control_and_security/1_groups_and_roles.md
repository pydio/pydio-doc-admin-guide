_As described in the previous section, both Users and Groups are in fact attached with a canonical Role. As such, managing users, groups and roles are using very similar interfaces. Still these objects present some specificities._

### Users

Users and Groups are edited via the `Admin >> Identity Management >> People` menu. 

Here you can:

- Search for a user or group via the top search text input
- Create a new user or group via one of the top right corner buttons `+USER` or `+GROUP`
- Browse the directory
- Re-organise groups via drag and drops
- Edit or delete users or groups by clicking on the buttons that are on the right end of the user/group row.  

Once you have created a user (or when you come back from the user explorer), you are presented with a dialog that offers many options. Below is an explanation about the various fields and actions that can be done from this dialog.

#### User Information

**Change Password**: directly reset the password for this user.

**Lock Out User**: this directly disable this user and kills all her open sessions.

**Force Reset Password**: this user will have to define a new password. *CAUTION*: if the user is currently logged in, she will be directly presented with a pop-up dialog and will have to change her password immediatly. She will then be forced out and will have to log back in. Thus, any workin progress will be lost.

**Name**: the display name that is used accross the application and presented to other users.

**Email**: in case mailing is enable, we will send various email targeted at this user to this mail account.

**Language**: the **Pydio Cells** interface is fully internationalised: the language used for the various labels accross the application depends on this parameter on a user-by-user basis. Currently German, English, Spanish, French, Italian and Brazilian Portugues are provided out of the box. Thanks to the wonderful community around Pydio, more are soon to come.

**Default Repository**: Default workspace that will be displayed to the user just after login.

#### Profile and Roles

**User ID**: This is the human readable ID of this user and also her login. This cannot be changed once a user has been created.

**Special Profile**: Profiles are an additional layer of permissions granting some specific capacities to users, plus the ability to automatically apply any role to a given profile. Preset values are `standard`, `admin` and `shared`.

**Manage Roles**: As described in the previous sections, the Admin can manually apply any predefined role to any user. This is done via this interface, where roles can also be re-ordered. Group roles and canonical role are also displayed for the ease of understanding.

#### Workspaces Accesses

See the previous section. This is where admins will set up the ACLs for this user.

#### Application Pages

Similarly to workspaces, ACLs are used to manage access to specific pages of the application, namely the Homepage (welcome panel displayed generally just after login, with user last activity and global search engine) and the Settings panel.

### Groups

Once you have created a group (or clicked on the `edit` button that is bound to an existing group), you are presented with a dialog that offers many options.
Most of them, namely the whole `Access Controls` and `Customize Parameters` sections, are the same that the ones used in the `User Editor`, excepted that configurations are applied at a `Group` level rather than at the `User` level.

In the Group section, you can find the group-specific following informations: 

**Group ID**: this is the full path of the edited group, last segment of the path being the ID of the group. This information cannot be changed after group creation.

**Group Label**: the display name for this group.

**Default Repository**: Same as for users

**Users Lock Action**: Here you can define an action that will be triggered automatically at users login. 

It can be logout (to lock out the users), pass_change (to force password change), or anything else. Warning, this is a powerful feature, handle with care.

### Roles

Again, most of the tabs contain the same info as for users and groups, e.g. the actual group informations. The "Role" menu still provides some specific fields : 

**Role ID**: Unique ID of this role, not editable

**Role Label**: Human-friendly label for this role

**Automatically Apply To...**: This field is used in conjunction with the "Profile" values of the users to automatically apply this role on any specific profile. This is particularly useful for denying some accesses and disabling some specific actions to the so-called "Shared" users.

**Always Override**: this switch is linked to the previous section about roles merging. As you have seen, roles can be applied to any users, and also re-ordered in the User editor. This switch would force the role to be always the last of the list.

### Parameters and Actions

The "Customize Parameters" section of the users/roles/groups editors is described in the next section.