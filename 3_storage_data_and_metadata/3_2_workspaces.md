
The rights to access a given set of data for one or more users is handled at a workspace level. Thus, once you have created a workspace, you will be able to grant read and/or write access to this workspace to your users.

You can assign these permissions individually, but you can also create Roles and Policies, (basically, a set of rules) to _dynamically_ grant accesses and permissions to a set of users. More on this in section 4 of this guide.

In this section, we will explain:

- How to create and manage workspaces
- The new _"Cells"_ concept that makes file and metadata sharing easy, userfriendly and very powerful.
- How to manage the specific _"Personal Files"_ folders

### Create and manage a workspace

### Cells: shared workspaces

### How do I create a "Personal" workspace ?

This is one of the out-of-the-box feature of Pydio Cells: each user have her own personal space, in which the other users cannot access. 

In contrary to former versions of Pydio, you do not have to configure anything to provide user with this capability. 

For the record, under the hood, such workspaces are stored under **TODO ...** in a sub folder named after current user login. 

Thus although it seem that all users access the "same" workspace, this workspace is actually always pointing to a different user, depending on the user logged.

~~ As you guessed it, this also requires the Workspace to be configured to automatically **"Create"** the personal folder when it is necessary. For this, you set the "Create" option to true in the workspace configuration.~~

### Technical Workspaces 