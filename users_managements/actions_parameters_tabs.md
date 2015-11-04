When you edit a group or a user, you are actually editing its underlaying role. Thus for the three of them (roles, groups, users), you will see the same tabs accessible : Common Info, ACLs, Actions and Parameters.

### Disabling actions
The Actions tab provides you a simple GUI to disable any action provided by any plugin for the rôle your currently editing. For example, let’s say you want to forbid download for all users who have the rôle “invited” that you created, do the following :

+ In the Actions tab of the rôle, open the Plugins Identifier chooser and select “access.fs”. The action name selector below will be automatically updated.
+ In the second selector (Action Name), select the Download action

[:image-popup:action_and_parameters_tabs/screenshot-2013-05-08-at-15-41-03.png]

+ In the third selector (Workspace Scope), you will select to which Workspace this specific setting will apply : either to all of them, or only one of them that you can choose, or specific subset that are “Shared Workspaces” : these are workspaces created by the users when they share a folder.
+ Finally, and don’t forget this step, **click on the “add action” button**. A new line should appear in the list below, describing the choice you just done.
+ Save the role.
Now if you log in as user to whom you have applied this role, you should see that the “Download” button has disappeared.

### Overriding default parameters values
In the last tab of the role editor, “Parameters”, you will be able to set up a predefined value for any parameter of any plugin, using a multiple-step selector as for the actions disabler.

For example, let’s assume have two groups of users, one that has a Quota of 50M by default, and another group that should have a quota limit of 200M for their “My Files” workspace. You have already configured the default Quota value to 50M.

+ Edit the second group and under Parameters, select the plugin identified by  “meta.quota”
+ Then in the second selector switch to “DEFAULT_QUOTA” parameter
+ And in the third selector, select the “My Files” workspace.
+ Click on “Add parameter” and Save the group.

You should now see something like this :

[:image-popup:action_and_parameters_tabs/screenshot-2013-05-08-at-15-49-15.png]