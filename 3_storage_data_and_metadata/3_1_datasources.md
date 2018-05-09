Datasources abstract the effective connection to a given type of data storage at a given address (that might give access to data via a proxy, typically to enable load balancing) using a specific driver.

When adding a new data repository to your Pydio Cells instance, you typically first define a datasource and then define workspaces that use it. 

Without diving to deep into the details, it helps to understand the basics of what is a datasource technically speaking. 

Roughly said, it is service that stands as the interface between your store (a local or remote file system, a S3 object storage...) and the Pydio Cells system.
It connects to the store on one hand and then exposes its files to the other internal services of Pydio Cells via a gRPC API. The takeaway here is that each datasource uses a distinct port to communicate with the rest of the application (typically 9001, 9002, 9003...).  

In this chapter, we will first guide you throught the process of creating a new datasource, either based on a file system or on a Amazon S3 Object repository.

It is also important to note that datasources provide a unique entry point for some important features, namely the versioning and the encryption that are defined at this level.

We will thus explain the way versioning is handle in Pydio Cells at the end of this chapter.

### Create and configure a datasource

In the below paragraphs, we dive into the specificity of the various datasources (drivers) but first, let's go through some common parameters you will have to set up, whatever the driver you select.

To create a new datasource go to:

`Admin Settings Page >> Storage >> + Datasource` (link on the top right corner of the page) 

[TODO add screenshot ?]

It opens a configuration Dialog.

#### Generic Parameters
**Datasource Identifier**


**Default Rights** : this settings is very handy to apply automatically an access right to all users for this workspace. If you set it e.g. to "Read", all new users will by default have a Read access to this workspace. This is particularly handy when you are binding Pydio to an external users directory : when a user logs for the first time, his Pydio counterpart is created at this moment, and using default rights is a good way to assign some workspace by defaults for all users.


**Default Rights** : this settings is very handy to apply automatically an access right to all users for this workspace. If you set it e.g. to "Read", all new users will by default have a Read access to this workspace. This is particularly handy when you are binding Pydio to an external users directory : when a user logs for the first time, his Pydio counterpart is created at this moment, and using default rights is a good way to assign some workspace by defaults for all users.

**Description** : this is now appearing as a legend in the Workspace selector.

**Alias** : this is automatically generated from the workspace label (like the "slugs" in Wordpress for example), and can be used as a workspace identifier for all the low-level operations : building the WebDAV server URLs, triggering an action via the Command Line, etc.

#### Filesystem Datasources

The most standard driver you can use is actually the File System datasource: it has the huge advantage of storing the files directly on a local or network file system, thus on the other way round, to read any existing data stored in a file system and empower it with Pydio Cells capabilities.

##### Defining peer address and path

** TODO peer and folder **

Here, the most important parameter will be the **PATH**. In one word as in thousand : **THE PATH PARAMETER MUST BE AN ABSOLUTE PATH** on the chosen peer, which means it must be a path starting from the root of the chosen server. 
The good news is that there are a couple of available keywords you can use in this field (and in any other field actually), that will be automatically resolved by the application to an absolute path (see below).

Possible values can then be:

+ `/var/repository/path/to/folder`
+ `C:UsersAdminDocuments`
** Check this **
~~+ `AJXP_INSTALL_PATH/../../`: AJXP_INSTALL_PATH is automatically replaced by the current Pydio installation folder
+ `AJXP_DATA_PATH/files`: AJXP_DATA_PATH is defined in the bootstrap_context.php file, it is pointing by default to AJXP_INSTALL_PATH/data, e.g. the folder that Pydio write into
+ `/home/AJXP_USER`: AJXP_USER is automatically replaced by the current user name, which answer the next question~~

##### Other driver parameters
Please see the plugin identity card for a complete description of all the options provided by the access.fs driver. Some are global (plugin options applicable to all workspaces), some are "instance", e.g. defined on a per-workspace basis.

Interesting options to note:

In the "Global part" (E.g. you have to go to Available Plugins > Workspaces Drivers > File System (Standard)), the **Real Size Probing** option may fix some issues with files of size greater than 2G.

In the instance part (options you see when creating a repository), the **Pagination** parameters will trigger a paginated view if a folder contains more than XXX number of items, the **File Creation Mask** is a chmod value applied at file creation, and the **Purge Days** can be setup to clear all files older than this number of days. This later option is not automatic, you have to set up a cron job (or see the Scheduler tool) to actually trigger the purge.


### Create a S3 Datasource connector

### Versioning

Versioning is provided out-of-the-box by Pydio Cells. In a few words, when versioning is turned on, everytime a user modifies a file, it creates a new version of this file that is then considered as the reference for this file. The old version is then stored in a technical data repository and eventually discarded depending on the pruning policies (more on this in the corresponding paragraph below).

Please, note that versionning has a cost in terms of the amount of data that is stored in your various data stores and have to be correctly configured and monitored in order to avoid exponential use of data space.

#### Default Pruning Policies

By default, the versioning policy of the Home Edition of Pydio Cells cannot be configured. The Admin can only choose to apply them or not _on a datasource basis_.

[TODO present each default policies]

#### Assigning a Given Policy to a Datasource 

Versionning can be enabled/disabled for a given datasource in the corresponding datasource editor that can be accessed via:
`Settings >> Storage >> _(chose a given data source to open the editor dialog)_ >> DataManagement >> Versioning Policy`.

#### [ED] Defining a Pruning Policy

In **Pydio Cells Enterprise Edition**, we have brought versionning one step further by giving you the option to define custom pruning policies that will fit to __your__ business specific needs and requirement.

The process of defining a new policy is quite straight forward and easy:

1. First go to your `Admin Settings Page`
1.     

