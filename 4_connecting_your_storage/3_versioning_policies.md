Versioning is provided out-of-the-box by Pydio Cells. In a few words, when versioning is turned on, every time a user modifies a file, it creates a new version of this file that is then considered as the reference for this file. The old version is then stored in a technical data repository and eventually discarded depending on the pruning policies (more on this in the corresponding paragraph below).

Please, note that versioning has a cost in terms of the amount of data that is stored in your various data stores and have to be correctly configured and monitored in order to avoid exponential use of data space.

When defining a datasource, you can pick an available Versioning Policies to apply it to this datasource.

## Default Versioning Policies

Pydio Cells comes bundled with a default set of Versioning Policies, as described below:

| Policy Name | Description |
|---|---|
|Keep All |Always keep all versions of all files|
|Max 30 days| Keep all versions before 15 days, then only 10 versions, then delete all versions older that 30 days.|
|Regular Pruning|Will keep 10 versions in the last 10 minutes, in the last 3 hours, in the last day, in the last 10 days, and finally delete all after 30 days.|

@TODO SCREENSHOT OUT OF DATE
[:image-popup:4_connecting_your_storage/versioning_policies_interface.png]

## Listing and reverting to a version

As a user, if a file is versioned, it is possible to list a document revisions by right-clicking on any file and selecting "Versions". 

With this interface, it is easy to either download a specific file revision, or revert to this revision.

@TODO SCREENSHOT (USER-SIDE UX)

## [ED] Defining a Versioning Policy

_Note: The Home Edition comes with the default set of Versioning Policies and they cannot be edited._

In **Pydio Cells Enterprise Edition**, we have brought versioning one step further by giving you the option to define custom pruning policies that will fit to __your__ business specific needs and requirement.

Go to your `Cells Console` then under **DataManagement > Storage use the "+ VERSIONING POLICY"** to create your own policy.

[:image-popup:4_connecting_your_storage/versioning_add_policies.png]

### Retention Rules

A couple of parameter give you great flexibility to keep storage footprint under control when it comes to retaining various revisions of your business files.

| Parameter | Description|
|---|---|
|Size Limits| Define a couple of limitations on a per-file basis or on a per-bucket basis to prune versions automatically when a given amount of storage space is reached.|
|Retention Periods|Define how versions will be pruned on a timely basis. Each retention period is composed of two parameters: <br/> - _Period Start_ is the time elapsed since a version was created <br/> - _Max number of versions_ is number of versions to keep during this period. Special numbers are -1 to keep all versions, or 0 to remove all versions.|
|After original file deletion|Define what to do with versions after a file is totally removed from a datasource (see below).|

### Storage location

By default, all policies use the specific "versions" internal datasource. If you wish to store versions inside another storage or volume, you may create your own internal datasource, and use it as a storage for your custom versioning policies. 

Only "internal" datasources can be used for storing versions, as they are more silent than standard datasources by avoiding events generation on resources creation or modification. However, as other datasources, they can be encrypted to make sure files revision are encrypted. 

Creating multiple versions datasources and their associated policies can be handy to shard data accross multiple storage (e.g store versions of each datasource inside separate storage).

## Restoring versions for permanently deleted files

Each versioning policy provides a setup to define how to handle kept versions when a file is totally removed from a datasource. There are three possible approaches :
  - **Backup all existing versions**: will move all existing versions (inside the version datasource) to a dedicated folder
  - **Backup most recent version**: will only keep the very last version (should correspond to the deleted file)
  - **Delete all existing versions**: will remove versions

When choosing one of the two first options, versions that are to be kept are moved inside the versions datasource under a dedicated `$DELETED$` folder. The path to the original file starting from the root is re-created under this folder, so that administrators can eventually find and restore a deleted file in-behalf of a user (generally in panic). 

#### Restoration example

For instance, let's assume user "john" permanently removed a file `important-file.doc` located in his Personal Files workspace under `Todos/Done` folder. 

On-demand, an administrator can create a temporary access to the "versions" datasource by creating a workspace that points to the root of this datasource. Make sure to not set any default rights on this workspace, otherwise all users may have the ability to crawl the whole versions datasource! 

Going through this workspace, the administrator will then be able to access to  
`$DELETED$/personal/john/Todos/Done/important-file.doc` and either manually move it to another workspace, or download it and send it back to happy john!
