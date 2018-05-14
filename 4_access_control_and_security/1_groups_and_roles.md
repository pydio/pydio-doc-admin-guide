
In the second chapter of this guide, you learned how to create users, groups and roles.
Now that everything is correctly setup and configured, we will go a step further to show you the advanced capabilities of *Pydio Cells*'s user management system that goes way beyond defining a bunch users and their access rights. 


### Main Concepts
 
The identity management features of Pydio Cells rely on an **internal user directory** that is implemented using a database and an index.
Basically, it means that a user (or a group or a role) is also a generic *Node* in the system. 
Thus, it can be shared, assigned some ACLs or policies or enriched with metadata.

#### Users

A user represent a single person or entity. The user _Object_ provides first and foremost the authentication within the system. 

In addition to her group and assigned roles, main properties of a user are her `username` (the login she uses to connect to the system) and a technical Unique ID (that is a `String UUID`) 

It can also be enriched with metadata, like various information (name, photo...), activities and audit logs.

#### Groups

Groups are a convenient way to organise users hierarchicaly.
A group is defined by its path, for instance `/management/directors` and can be nested under another group. The unique identifier of a group is the last part of its path.

When installing the application, a single default group is present: the `root` group that as a specific `/` path. The `root` group is the ancestor of all users and groups.

A user can only be part of one group and is characterised by her login and the path of her group: typically:  `/management/director/jane`

#### Roles

The role abstraction is used by the system when you define authorisations and policies. Each user and group is always at least attached with a _canonical_ role that represents it.
Thus, for instance, when you assign specific permissions to a given user, these permissions are assigned under the hood to the canonical role that have the same name.

Roles also add an indirection level that permits the definition of dynamic _groups_ that will evolve depending on external conditions.

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

#### Team Visibility





This is a specific Enterprise Distribution feature: starting with Pydio 8, you can import users massively using a Comma Separated file.

### The inheritance

Practically, one will probably not want to define X roles and apply them one by one to the users. But the other power of this object is that it is shared by Groups and Users as well. Which means a user automatically will provide its own role (internally name AJXP_USER_/userlogin), and if he belongs to a group, this group will also provide a specific group (named AJXP_GRP_/groupPath/groupName).

You can see that editing a user or a group, you’ll find the same tabs ACL, Actions and Parameters that you would find in the role edition. They correspond to the specific roles created for each object ( a group or a user).

![role interface](/images/4_access_control_and_security/roles_interface.png)

Once configured, the various roles that are applicable to a user will be « added » in the following order : first the groups roles (if a user belongs to a subgroup, then the parent group role is applied, then the subgroup role), then the administrator-defined roles, then the proper user role. This is summarized in the figure below :

![roles representation](/images/4_access_control_and_security/roles_representation.png)

For example, a user U1 from group G1, to whom one have applied a specific role R, will then have a « merged » role that result in the addition of the following :

**AJXP_GRP_/G1 + R + AJXP_USR_/U1 = user runtime role.**


### Importing existing users

This section will describe ways to connect Pydio Cells to your existing users directories.
