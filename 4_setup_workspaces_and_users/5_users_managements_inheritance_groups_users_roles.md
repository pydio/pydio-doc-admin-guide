### Using Roles in Pydio

Role can be found in "Workspaces & Users" > "People"; you can see in the left of the panel 2 tabs ( Groups and Roles ). When you click on "Roles" 2 new buttons can be found at the bottom of the panel ( + Role and Edit ).

[:image-popup:4_setup_workspaces_and_users/users_managements_group_user_create_role_update.png]

A role contains the following informations:

+ **Workspaces Accesses** (Access Controls): a list of rights giving or denying an access to each workspace defined in the application. Internally, the right string can be « read », « write », a combination of these two, or « deny » (to force clearing a previously set value in another role).

+ **Application Pages** you can choose if give them access to pages such as *My account,welcome page, ...*

+ **Application Parameters** (Customize Parameters): a tree of  disabled plugins actions, that can be as well disabled for all workspace or only a precise list. Parameters and Actions can in some case be applicable only for a given list of workspace.

+ **All workspaces parameters** (Customize Parameters): a tree of plugins parameters values that can be applied to all of the workspaces.

+ **Per-workspaces parameters** (Customize Parameters): a tree of plugins parameters values that can be applied to a given set of workspace. If you want to edit them for all workspace you must go to **All workspaces parameters**.


You can see the various categories to the corresponding tabs of the edition interface in the Settings workspace (see screenshot below).

[:image-popup:4_setup_workspaces_and_users/users_managements_group_user_role_ws.png]
Figure 1 Role Edition Interface (Whole workspace)

[:image-popup:4_setup_workspaces_and_users/users_managements_group_user_role_sub_ws.png]
Figure 2 Role Edition Interface (Sub folders)

The power of the « Role » object is that it is a shared concept that is « additive »: a sequence of n roles can be « added » to create a new role, each role n in turn overriding the various parameters of the role n-1.  That way, one can define for example a given plugin parameter value for in role1, then refine this value in Role2, and if a user has these two roles in this order, the latest value will be taken into account.

### Groups and users « runtime » role
Practically, one will probably not want to define X roles and apply them one by one to the users.  But the other power of this object is that **it is shared by Groups and Users** as well. Which means a user automatically will provide its own role (internally name AJXP_USER_/userlogin), and if he belongs to a group, this group will also provide a specific group (named AJXP_GRP_/groupPath/groupName).

You can see that editing a user or a group, you’ll find the same tabs "Workspaces Accesses", Actions and Parameters that you would find in the role edition. They correspond to the specific roles created for each object ( a group or a user).

You can add a role in the section of the user editing : **"Workspaces & Users" > "Users" > "Roles tab" > "+ ROLE"**

[:image-popup:4_setup_workspaces_and_users/users_managements_user_edit_roles.png]

Once configured, the various roles that are applicable to a user will be « added » in the following order: first the groups roles (if a user belongs to a subgroup, then the parent group role is applied, then the subgroup role), then the administrator-defined roles, then the proper user role. This is summarized in the figure below:

For example, a user U1 from group G1, to whom one have applied a specific role R, will then have a « merged » role that result in the addition of the following:

**AJXP_GRP_/G1 + R + AJXP_USR_/U1 =  user runtime role.**

#### _Use Cases_

This « refinement » process is very flexible, and allows you to configure the accesses, parameters and actions of each plugin, for each workspace, at a very fine level. Generally, the « Groups » is a right place to preconfigure a couple of parameters, but sometime you’ll want to disable some specific action for a given user, in which case you’ll edit directly the user role.

### Specific profiles
Another interesting feature of the roles one creates manually in the application is that they can automatically be applied to a given profile of users. There are currently four profiles of users: Standard, Administrator, Shared and Guest.

The « shared » profile is automatically applied to « external » users created by the internal users. If you want that these user have a specific role, you can create a role and set it to be  automatically applied to all shared users.
