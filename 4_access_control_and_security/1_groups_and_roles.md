
In the second chapter of this guide, you learned how to create users, groups and roles.
Now that everything is correctly setup and configured, we will go a step further to show you the advanced capabilities of *Pydio Cells*'s user management system that goes way beyond defining a bunch users and their access rights. 

### Main Concepts
 
The identity management feature of Pydio Cells relies on an **internal user directory** that is implemented using a database and an index.
Basically, it means that a user (or a group or a role) is also a generic *Node* in the system. 
Thus, it can be shared, assigned some ACLs or policies or enriched with metadata.

#### Users

A user represent a single person or entity. The user _Object_ provides first and foremost the authentication within the system. 

In addition to her group and assigned roles, main properties of a user are her `username` (the login she uses to connect to the system) and a technical Unique ID (that is a `String UUID`) 

A user can also be enriched with metadata, like various information (name, photo...), activities and audit logs.

#### Groups

Groups are a convenient way to organise users hierarchicaly.
A group is defined by its path, for instance `/management/directors` and can be nested under another group. The unique identifier of a group is the last part of its path.

When installing the application, a single default group is present: the `root` group that as a specific `/` path. The `root` group is the ancestor of all users and groups.

A user can only be part of one group and is characterised by her login and the path of her group: typically:  `/management/director/jane`

#### Roles

The role abstraction is used by the system when you define authorisations and policies. Each user and group is always at least attached with a _canonical_ role that represents it.

Thus, for instance, when you assign specific permissions to a given user, these permissions are assigned under the hood to the canonical role that have the same name.

Roles also add an indirection level that permits the definition of dynamic _groups_ that will evolve depending on external conditions.

#### Profiles

*TODO*

#### External Directories

As already quoted, **Pydio Cells** comes with an embedded internal user repository that must be used.

Yet, you can transparently plug your existing directories (currently LDAP and Active Directory Server) to the system. Users will be synchronized to the internal Pydio Directory (rather than using a binding to the master external directory at runtime). 

#### Access Control Lists

ACLs are first used to statically define permissions. You can pick up a role, a group or user and define what data can be accessed and modified.

It is important to note that data can be files or folders but also other users and group (more on this in the team visibility section below).

**An important take away**: when multiple ACLs (either static or dynamic) can be applied on a single `Node`, either because they are attached directly to this `Node` or to one of its ancestor,they are merged following the rule of _Deny by Default / Allow By Exception / Denial Wins_

For instance for `READ` access this translates to:

- by default a `Node` cannot be seen
- a single authorize on the `Node` or one of its ancestor is enough to give read access to this `Node`
- a single explicit deny, on the `Node` or one of its ancestor will take precedence and prevent access.
 
#### Policies

Policies are a powerful feature of **Pydio Cells** used to dynamically assign ACLs depending on the context (logged in user, day and time of the week, client IP address...) and a set of rules that can be scripted and combined together.

Once a Policy template has been defined via the `Admin Settings >> Security Policies` page, you can apply it instead of using static `READ / WRITE / DENY`.

For instance when editing a group via `Admin Settings >> People >> (Pick up a group for edit) >> Workspaces Accesses`, you can click on the `more` icon that is on the left of the check box of one of the workspace and choose a custom policy.


### Managing People (Users and Groups)

There are 2 ways of managing users and groups: from an admin point of view, via the `Admin Settings >> People` page, or as a regular user using the address book that can be typically accessed via `(Top Menu More Button) >> Address Book`.

#### From the admin perspective

_You have to be an *Administrator* e.g. to use an acount which has been assigned the profile *Administrator* to use this feature._

So you have to go to the `Admin Settings >> People` page.

Here you can:

- Search for a user or group via the top search text input
- Create a new user or group via one of the top right corner buttons `+USER` or `+GROUP`
- Browse the directory
- Re-organise groups via drag and drops
- Edit or delete users or groups by clicking on the buttons that are on the right end of the user/group row.  

##### User Editor

Once you have created a user (or when you come back from the user explorer), you are presented with a dialog that offers many options. Below is an explanation about the various fields and actions that can be done from this dialog.

###### User Information

**Change Password**: directly reset the password for this user.

**Lock Out User**: this directly disable this user and kills all her open sessions.

**Force Reset Password**: this user will have to define a new password. *CAUTION*: if the user is currently logged in, she will be directly presented with a pop-up dialog and will have to change her password immediatly. She will then be forced out and will have to log back in. Thus, any workin progress will be lost.

**Name**: the display name that is used accross the application and presented to other users.

**Email**: in case mailing is enable, we will send various email targeted at this user to this mail account.

**Language**: the **Pydio Cells** interface is fully internationalised: the language used for the various labels accross the application depends on this parameter on a user-by-user basis. Currently German, English, Spanish, French, Italian and Brazilian Portugues are provided out of the box. Thanks to the wonderful community around Pydio, more are soon to come.

**Default Repository**: TODO

**Shared Users Limit**: TODO 

###### Profile and Roles

**User ID**: This is the human readable ID of this user and also her login. This cannot be changed once a user has been created.

**Special Profile**: TODO

**Manage Roles**: TODO

###### Workspaces Accesses

This page presents a list of known workpaces.

For each workspace, we can manually define permission for this user by simply checking or unchecking the corresponing box. This also applies to groups and roles which editors contains a similar table.

When a workspace contains subfolder, you can extends the corresponding line by clicking on the `+` sign that is next to the workspace label.

You can then assign static permissions on a folder by folder basis.

When doing so, you have one more option that is represented by an additionnal `children` check box. The helper message that appears when checking the box are very helpful and self-explanatory, yet the basic idea of this check box is to choose if :

- permissions for this folder are defined at this level
_or_
- if they are delegated to the children level and so on.

In addition to the check box, you can also open a popup list by clicking on the 3-dots `more` button that lays on the left of the boxes and then choose a given policy template that will be applied for this user/group/role on this workspace/subfolder, rather than explicitely defining permissions.

You can thus, for instance, only enable access and modification of a whole workspace during office hours or from a given IP address.  

###### Application Pages

**TODO**

###### Shares 

This page displays a list of the Shares (Cells) that are **owned** by this user.
This list is one of the few non dynamic views of **Pydio Cells** and must be explicitely refreshed upon request.


##### Group Editor

Once you have created a group (or clicked on the `edit` button that is bound to an existing group), you are presented with a dialog that offers many options.
Most of them, namely the whole `Access Controls` and `Customize Parameters` sections, are the same that the ones used in the `User Editor`, excepted that configurations are applied at a `Group` level rather than at the `User` level.

Please, refer to the previous section of this guide to get more information on the subject.

Yet, the first section of the group editor, called `Summary`, is specific to the group level.

Here you can edit following information: 

**Group ID**: this is the full path of the edited group, last segment of the path being the ID of the group. This information cannot be changed after group creation.

**Group Label**: the display name for this group.

**Default Repository**: **TODO**

**Users Lock Action**: Here you can define an action that will be triggered automatically at users login. 

It can be logout (to lock out the users), pass_change (to force password change), or anything else. Warning, this is a powerful feature, handle with care.

#### From the user perspective

In addition to the user management features for the system administrator that have been presented in the previous section, users and groups can also be accessed, used and partly configured by regular users using the `Address Book`.

Althought mainly targeted for end users, general understanding of this feature is helpful for Pydio Cells administrator. 

##### Using Address Book

The `Address Book` is mainly used to select a user or a group, typically when configuring a `Cell`. In such case, the address book opens a popup list where a user can choose a category between `Shared Users`, `Teams` or `Directory` and then pick up one of the defined user or group.

##### Managing the Address Book

More generally, a user can explicitely open her address book via her settings menu (the 3 dot "_more_" menu icon that is next to one user's display name in the top left corner).

A dialog opens with following sections:

###### Shared Users

###### Teams

###### Directory







