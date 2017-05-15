<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 7. Looking for <a href="https://pydio.com/en/docs/v8/groups-admin-and-delegation">Pydio 8 docs?</a></span>
</div>

Introduced in V5 groups were a longly awaited feature. The main benefit can been seen at two levels

+ Using the mechanism explained in the previous section, it is now possible (if not “easy”) to totally customize a set of plugins parameters values as well as disable a specific set of actions for a given group of users. For this, you use the “group role” accessible by clicking on the small pencil in "Workspaces & Users" > People.

[:image-popup:4_setup_workspaces_and_users/users_managements_delegation_group.png]

+ Inside a group, you can assign the administrator profile to any user. This user will not be a “superadmin”. She will only have access to a subset of the configurations data, and this limited to the current group :
    - Users / Roles : create users INSIDE the group only, create roles that will be only available for this group.
    - Workspace : create workspace from the templates the super admin has configured, available only inside this group.

[:image-popup:4_setup_workspaces_and_users/users_managements_delegation_user_group.png]

The image above shows the “Settings” view a group administrator has : only a subset of the features are available to him.
