Versioning is provided out-of-the-box by Pydio Cells. In a few words, when versioning is turned on, everytime a user modifies a file, it creates a new version of this file that is then considered as the reference for this file. The old version is then stored in a technical data repository and eventually discarded depending on the pruning policies (more on this in the corresponding paragraph below).

Please, note that versioning has a cost in terms of the amount of data that is stored in your various data stores and have to be correctly configured and monitored in order to avoid exponential use of data space.

When defining a datasource, you can pick on of the available Versioning Policies to apply them to this datasource.

### Default Versioning Policies

Pydio Cells comes bundled with a default set of Versioning Policies, as described below:

[:image-popup:4_connecting_your_storage/version_policies_interface.png]

- **Keep All**: never delete any version of any files
- **Max 30 days**: Keep all versions before 15 days, then only 10 versions, then delete all versions older that 30 days.
- **Regular Pruning**: Will keep 10 versions in the last 10 minutes, in the last 3 hours, in the last day, in the last 10 days, and finally delete all after 30 days.


### [ED] Defining a Versioning Policy

_Note: The Home Distribution comes with the default set of Versioning Policies and they cannot be edited._

In **Pydio Cells Enterprise Distribution**, we have brought versioning one step further by giving you the option to define custom pruning policies that will fit to __your__ business specific needs and requirement.

Go to your `Settings Page` then under **DataManagement > Storage use the "+ VERSIONING POLICY"** to create your own policy.

[:image-popup:4_connecting_your_storage/adding_versioning_policies.png]

- **Storage**: Will define where the versions will be stored. The default values will use the default datasource (pydiods1 defined at install) under a specific "versions" bucket.
- **Size Limits** Allows you to define a couple of limitations on a per-file basis or on a per-bucket basis to prune versions automatically when a given amount of storage space is reached.
- **Retention Periods** Define how versions will be pruned on a timely basis. Each retention period is composed of two parameters:
  - _Period Start_ : time elapsed since a version was created
  - _Max number of versions_: number of versions to keep during this period. Special numbers are -1 to keep all versions, or 0 to remove all versions.
