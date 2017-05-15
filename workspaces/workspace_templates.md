<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 7. Looking for <a href="https://pydio.com/en/docs/v8/">Pydio 8 docs?</a></span>
</div>

### The case
Creating workspaces ones by ones as admin is OK in most cases, but sometimes you will want to delegate this to other users : either end-users themselves, because creating a “dropbox” workspace requires them to configure their own credentials or API key, or potentially “sub-admins” : groups administrators.

In that case, you would probably preset most of the parameters values and features ones can use for these workspaces, and let the other party enter only the necessary parameters, in a limited way.

Or simply you find yourself creating new workspaces often with the same whole set of parameters, only a couple of them changing (path, default rights), and you’d like to make this more straightforward.

### Creating Templates
By creating workspace templates, you can preset most of the workspace parameter, and depending on who will finally create an actual WS from it, either let him override your values, or simply let him fill the very few parameters you let “open”.

When you create a template, the standard parameters forms have a “checkbox” inserted for each field. All fields checked will be considered “set”. This is allows you to set a field to an empty value, and still consider it’s set. Thus you will setup the template exactly as you would for a workspace.

#### _Template specific variable : AJXP_ALLOW_SUB_PATH_

You can use in the various fields (like the Path field) the standard keywords (AJXP_INSTALL_PATH, AJXP_DATA_PATH, AJXP_USER, AJXP_GROUP_PATH), but also a specific template keyword : AJXP_ALLOW_SUB_PATH. This keyword is often the key to your sandboxing nightmares : by defining a path with like the following /path/to/folder/AJXP_ALLOW_SUB_PATH, the parameter can be later on set with a path that will always be prepended (and not resolved of course) by /path/to/folder/. That way a user may enter “myown_folder”, the final workspace path will point to /path/to/folder/myown_folder.

If the user enters something like “../../myown_folder”, the path will always be cleaned up and the result will be the same /path/to/folder/myown_folder.

#### _Templates specific parameters_

Under the “Template Options” section, you will find some template specific parameters:

+ **Allow to user** : combined with the “Let users create workspace” option of the global Configurations Management panel, if set to yes, this will appear in the users workspace create dialog.
+ **Allow to group admins**: if set to yes, groups administrators will be allowed to create workspaces using this template.
+ **Default label** : applies when user create a WS : will set the WS label by default.
+ **Small Icon** : define a specific icon for the workspace menu
+ **Big Icon** : define a specific icon for the create workspace dialog

### Letting users create their own workspaces
If you check the “Let users create workspaces” value in the Configurations Management panel, and if you define some templates with the parameter “Allow users” set to true, the users will have an additionnal action at the bottom of their Workspaces list, telling “Create workspace…”. In there , they will be able to select a predefined template, enter the last parameters still not set, and create the workspace. It will be “personal”, only them will be able to access this workspace.

[:image-popup:workspace_templates/12-template-workspace.png]
