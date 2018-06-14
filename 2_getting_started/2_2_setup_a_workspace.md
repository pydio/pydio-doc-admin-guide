_In Pydio Cells, Workspaces provide the "basic unit" for assigning access rights to users. But unlike in previous versions, they are now fully decoupled from the actual data location, as this is handled via the "DataSources"._

Datasources are internally exposed via a "tree" service that dynamically aggregates all datasources into one big virtual filesystem.
A workspace is then a view on a business relevant subpart of this virtual tree, with specific permissions and metadata. 

### Default Datasources

After installation, Pydio Cells comes with a set of default datasources that are directly usable to create workspaces:

* **pydiods1** : a generic datasource that can be used to create workspaces shared accross users
* **personal** : a user-oriented datasource where the personal files of each users are stored
* **cells** : another user-oriented datasource used when users create empty cells with no specific folders in them.

The 2 last ones are used in cunjonction with "Template Paths" nodes: these paths contain contextual information (like the user name) and are resolved at runtime to point to the correct location.

The data is located by default under `/home/{pydio_user}/.config/pydio/cells/data` unless you have modified this path using the Advanced settings of the installer. You should find the corresponding folders on your server (/pydiods1/, /cells/, /personal/). If you want to feed a datasource with existing data, just copy it inside the folder, then hit "Re-Index DataSource" in the settings panel. This will detect the new data and make it available for creating workspaces.

You can also create you own specific datasources, [more on this in the storage chapter of this guide](/en/docs/cells/v1/managing-datasources)

### Managing workspaces

To create a workspace go to **Data Management > Workspaces**, then click on **+WORKSPACE** located on the right.

Then this menu will appear :

[:image-popup:2_getting_started/create_workspace.png]

1. **Root nodes** : Using the auto-complete field, pick one or more nodes ( = files or folders ) from the available datasources to compose your workspace contents. The **cells** and **my-files** are specific paths that can only be selected (you cannot browse their children) and will be resolved automatically at run-time. Note that you can add as many nodes as necessary.
1. **Label** : Human friendly name of the workspace, as seen by end users.
1. **Description** : Optionnal hint for the end user.
1. **Default Rights** : This is applied to the "Root Role" of the application, that is to all users unless one of their roles overrides this behaviour. You can choose the default `Read and Write` option for this workspace.
1. **Alias** : You should generally leave this empty, a unique alias will is generated upon creation. Still, you can pick your own alias (make sure to use only alphanumerical characters and more generally URI-compatible characters).

After its creation, you can modify a workspace by going to **Data Management > Workspaces** and clicking on the workspace of your choice.

[:image-popup:2_getting_started/modify_workspace.png]

### Providing access rights to your workspace

Once a workspace is created, use the Users, Groups and Roles interfaces to grant effective permissions to your users as explained [in the next chapter](/en/docs/cells/v1/users-roles-and-groups).