<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 7. Looking for <a href="https://pydio.com/en/docs/v8/setup-workspace-and-assign-rights">Pydio 8 docs?</a></span>
</div>

This section has been made to quickly introduce you to workspaces.
More information is available inside [this section](../4_setup_workspaces_and_users.md)

What is a workspace?

With Pydio, in order to manage your data, you can create workspaces.
A workspace is like a directory within it's storage area.
You can manage who can access a workspace, define specific rules, and define the folder access inside.

### Create a workspace

To create a workspace, go to "Workspaces & Users" > "Workspaces" then click on the "+WORKSPACE" button:

1 - Select a template or driver:
- Pick Driver

2 - Workspace options:
- Input the workspace name inside "Label" field
- Input the workspace description inside "Description" field
[:image-popup:2_getting_started/workspace_configure_global.png]

3 - Choose Storage
- Pick "File System (Standard)"
- Input inside the "Path" field "AJXP_DATA_PATH" (it will automatically go to your Pydio data folder)
[:image-popup:2_getting_started/workspace_configure_driver.png]

4 - Save to add features
- Click on the save button

Congratulations, you have now created your first workspace with Pydio Enterprise dashboard.

### Create a user and assign rights

To create a user, go to "Workspaces & Users" > "Users" then click on the "+USER" button:

Fill the primary information inside the popup.
[:image-popup:2_getting_started/user_create.png]

Now you should see the detailed settings for the user.
These fields are optionals, but feel free to fill them.


Now go to "Workspace Accesses" tab and there you can allow your user to access your workspace the way you want:
[:image-popup:2_getting_started/user_permissions.png]
