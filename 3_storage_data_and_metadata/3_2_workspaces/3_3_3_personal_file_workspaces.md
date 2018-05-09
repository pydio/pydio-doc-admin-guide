### How do I create a "Personal" workspace ?

This is one of the out-of-the-box feature of Pydio Cells: each user have her own personal space, in which the other users cannot access. 

In contrary to former versions of Pydio, you do not have to configure anything to provide user with this capability. 

For the record, under the hood, such workspaces are stored under **TODO ...** in a sub folder named after current user login. 

Thus although it seem that all users access the "same" workspace, this workspace is actually always pointing to a different user, depending on the user logged.

~~ As you guessed it, this also requires the Workspace to be configured to automatically **"Create"** the personal folder when it is necessary. For this, you set the "Create" option to true in the workspace configuration.~~