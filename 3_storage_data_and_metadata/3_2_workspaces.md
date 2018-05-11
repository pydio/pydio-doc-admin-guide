
Now that you have defined your datasources, you can merge and organise the various views over your data at a business level. That is the main goal of workspaces.

Workspaces are top-level folders helping you organize your data. They may be accessed only by a given user or shared accross many people.

To simplify, a workspace can simply and cannonically expose a subtree of any of the filesystems that is provided by one of your datasources, but can also gather transparently data from heterogeneous sources or mounting points in a single data source: in some case, you might see it like a folder containing symbolic links on a linux-like file system.

In this section, we will introduce the most widely used types of workspace namely:

- basic workspaces that refers only to a subtree of a datasource
- shared workspaces a.k.a _Cells_ that makes file and metadata sharing easy, userfriendly and very powerful
- personal file workspaces, and more generally speaking dynamic workspaces
- technical workspaces.

In previous versions of Pydio, right management (e.g. giving permissions to a set of users to access and or modify data) was only done at the workspace level.

With Pydio Cells, you now have much more flexibility and it is possible to define finer grained permissions that can be assigned basically at 3 levels:

- Workspace level
- Cells level
- Node level (e.g. directly on a given file or folder)

Internally, permissions are now bound to a generic _Node_ object that can be any of the 3 concepts listed above.  You can assign these permissions individually, but you can also create via Roles and Policies, (basically, a set of rules) to _dynamically_ grant accesses and permissions to a set of users. 

Considering this, the workspace concept is less central to the permission management and you should refer to chapter 4 of this guide that focus on security to be able to take advantage of the latest features brought by this release.

### Simple workspace

As you have already seen in chapter 2 of this guide, it is straight forward and easy to define a simple workspace: please refer to section #2.2 for a step by step guide.

You then have a direct access to the root of the underlying datasource and can start using it.

A few precisions:

*About the Default Rights*: the default rights is a specific policy that will be applied on the workspace by default **if no other policy applies**.
It is a dynamic rules, thus if you create a new workspace and select the Read-Write option, all your user will see this workspace appears in there workspace list (in the left menu) and can start uploading and modifying files.
If you then edit the workspace and select Read-only permission, this will also directly taken into account.

*About the Recycle Bin*: **TODO**

### Cells: shared workspaces

Cells are a new central concept in this version of the Pydio tool and are a great way to simply and securely share data among users.

Technically speaking, creating a cell tranparently adds an indirection level that enable the definition of set of heterogeneous data (either files or folder, even from various datasources).
You can then define permissions via Rules and Policies directly at this level, e.g. on this data set, as if the underlying data was really present under this virtual folder.

**GOTCHAS** TODO add a few problematic corner cases while using cells?


### Personal workspaces

This is one of the many out-of-the-box cool features of Pydio Cells: with no further configuration, each user have her own personal space, in which other users cannot access. It is very similar to the `home` folder concept that you find in various operating systems.

In contrary to former versions of Pydio, you do not have to configure anything to provide user with this capability. 

For the record, under the hood, such workspaces are stored under **TODO ...** in a sub folder named after current user login. 

Thus although it seem that all users access the "same" workspace, this workspace is actually always pointing to a different user, depending on the user logged.

~~ As you guessed it, this also requires the Workspace to be configured to automatically **"Create"** the personal folder when it is necessary. For this, you set the "Create" option to true in the workspace configuration.~~

### Technical Workspaces 