The most standard driver you can use is actually the File System driver : it has the huge advantage of storing the files directly on the local file system, thus on the other way round, to read any existing data stored in a file system and empower it with Pydio capabilities.

### Defining the path
Here, the most important parameter will be the **PATH**. In one word as in thousand : **THE PATH PARAMETER MUST BE AN ABSOLUTE PATH**, which mean it must be a pass starting from the root of the server. The good news is that there are a couple of available keywords you can use in this field (and in any other field actually), that will be automatically resolved by the application to an absolute path (see below).

Possible values can then be:

+ `/var/repository/path/to/folder`
+ `C:UsersAdminDocuments`
+ `AJXP_INSTALL_PATH/../../`: AJXP_INSTALL_PATH is automatically replaced by the current Pydio installation folder
+ `AJXP_DATA_PATH/files`: AJXP_DATA_PATH is defined in the bootstrap_context.php file, it is pointing by default to AJXP_INSTALL_PATH/data, e.g. the folder that Pydio to write into
+ `/home/AJXP_USER`: AJXP_USER is automatically replaced by the current user name, which answer the next question

### How do I create a "Personal" workspace ?
This is a frequent usage of Pydio: you want each user to have her/his own personal space, in which the other users cannot access. Instead of manually creating one workspace per user (which would be awful), you simply create one UNIQUE workspace with the Path parameter using the `AJXP_USER` keyword : this keyword will be dynamically replaced by the current user login, thus although it seem that all users access the "same" workspace, this workspace is actually always pointing to a different user, depending on the user logged. You can see that this keyword is used for the default "My Files" workspace bundled with the application : the path is `AJXP_DATA_PATH/personal/AJXP_USER`.

As you guessed it, this also requires the Workspace to be configured to automatically **"Create"** the personal folder when it is necessary. For this, you set the "Create" option to true in the workspace configuration.

### Other driver parameters
Please see the plugin identity card for a complete description of all the options provided by the access.fs driver. Some are global (plugin options applicable to all workspaces), some are "instance", e.g. defined on a per-workspace basis.

Interesting options to note:

In the "Global part" (E.g. you have to go to Available Plugins > Workspaces Drivers > File System (Standard)), the **Real Size Probing** option may fix some issues with files of size greater than 2G.

In the instance part (options you see when creating a repository), the **Pagination** parameters will trigger a paginated view if a folder contains more than XXX number of items, the **File Creation Mask** is a chmod value applied at file creation, and the **Purge Days** can be setup to clear all files older than this number of days. This later option is not automatic, you have to set up a cron job (or see the Scheduler tool) to actually trigger the purge.
