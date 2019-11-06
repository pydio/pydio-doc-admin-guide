### Role as a central concept

#### Roles

The role abstraction is used by the system when you define authorizations and policies. A role is composed of a set of the following : 

- **Access Control List**: this will attach a permission for this role to access any Node of the global tree (either via a Workspace or a Cell and their root nodes, or directly applied on specific files or folders). This permission can either be fixed (Read, Write) or dynamically computed at runtime by a Security Policy (see [Security Policies section](/en/docs/cells/v1/security-policies) ).
- **Actions**: enables or disables available actions in the GUI
- **Parameters**: override the global values of a parameter

Roles can be created manually by an Administrator, and then applied to any users. Roles are also used under the hood to define users Teams.

#### Groups

Groups are a convenient way to organise users hierarchicaly. A group is defined by its path, for instance `/management/directors` and can be nested under another group. The unique identifier of a group is the last part of its path.

When installing the application, a single default group is present: the `root` group that as a specific `/` path. The `root` group is the ancestor of all users and groups.

**Each group is attached with a _canonical_ role** that takes the group Uuid as Uuid.

#### Users

A user represent a single person or entity. The user provides the authentication within the system. 

A user can only be part of one group and is characterised by her login and the path of her group: typically:  `/management/director/jane`

The main properties of a user are her `username` (the login she uses to connect to the system) and a technical Unique ID (that is a `String UUID`). A user can also be enriched with metadata, like various information (name, photo...), and the user Login is used in the activities and audit logs.

**Each user is attached with a _canonical_ role** that takes the user Uuid as Uuid.

Additionally, an ordered list of Roles can be attached to any users.

#### Computing Effective Role

As you probably understood by now, when a user is logged in, this user may have a certain number of Roles applying to her and used compute her actual role:

- If the user is part of nested groups /management/directors, she will inherit each Group Role starting from root:
  - _RootRole_ defined for "/"
  - _ManagementRole_ defined for "/management"
  - _DirectorsRole_ defined for "/directors"
- If the user has some arbitrary roles applied, e.g. "Subscriber" applied by admin and "Team of John" because she's part of a user team:
  - _SubscriberRole_
  - _TeamOfJohnRole_
- Finally the user always has her own canonical role
  - _CanonicalUserRole_

The rules that will apply when trying to detect if the user has a right to access a node or should see a given action for example will be computed by **merging the various roles values in this order**: from top (always _RootRole_ first) to bottom (always _CanonicalUserRole_ last).

#### Examples

This model provides great flexibility to assign permissions on the Administrator side. Typical schemes can look like:

- Assign Read/Write on workspace Personal Files to Root Group (all users)
- Assign Read/Write access on workspace Accountants to group Accountants, same on workspace Engineers to group Engineers, etc...
- Assign Read access on workspace "Marketing Files" to Root Group, but Write access on this workspace to role "Marketing Editors". Then assign role "Marketing Editors" to a subset of users in various groups
- Deny Access on Personal Files to role "External Users", and make this role automatically applied to any user created by users (shared users),
- etc.

### Access Control Lists

Access Control Lists allows the Administrator to actually apply the strategies described above. In the various users/roles/groups editors, one can grant permissions on Workspaces or on workspaces subfolders in two ways: statically using simple Read/Write values, or dynamically using Security Policies (Enterprise Edition).

#### Static Accesses

The basic operation to assign right goes throught the "Read" / "Write" / "Deny" checkboxes. The first two will grant corresponding privilege to the currently edited object. The "Deny" operation is used to override a right that would have been applied by a role higher in the chain.

When editing ACLs, we recommend sticking to the Workspace level of granularity, to make your security model more maintainable. In some cases, you may require to directly assign rights at a folder level. For that you can use the "+" button next to the workspace name to list the workspace children and assign rights accordingly.

#### [ED] Policies

Policies are a powerful feature of Pydio Cells used to dynamically assign ACLs depending on the context (logged in user, day and time of the week, client IP address...) and a set of rules that can be scripted and combined together.

Once a Policy template has been defined via the `Admin Settings >> Security Policies` page, you can apply it instead of using static "READ / WRITE / DENY".

For instance when editing a group via `Admin Settings >> People >> (Pick up a group for edit) >> Workspaces Accesses`, you can click on the `more` icon that is on the left of the check box of one of the workspace and choose a custom policy. See the [Security Policies](/en/docs/cells/v1/security-policies) section.

#### ACLs Merging Rules

When multiple ACLs (either static or dynamic) are applied on a single `Node`, either because they are attached directly to this `Node` or to one of its ancestor, they are merged following the rule of _Deny by Default / Allow By Exception / Denial Wins_

For instance for `READ` access this translates to:

- by default a `Node` cannot be seen
- a single authorize on the `Node` or one of its ancestor is enough to give read access to this `Node`
- a single explicit deny, on the `Node` or one of its ancestor will take precedence and prevent access.