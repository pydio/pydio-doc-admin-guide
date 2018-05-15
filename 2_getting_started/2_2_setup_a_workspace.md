_In Pydio Cells (like in Pydio), Workspaces provide the "basic unit" for assigning access rights to users. But unlike in previous versions, they are now fully decoupled from the actual data location, as this is handled via the "DataSources"._

### Default Datasources

Datasources will be exposed internally in the application via a "tree" service that dynamically aggregates all data into one big virtual filesystem. 

After initial installation, Pydio Cells comes with a set of datasources that are directly usable to create workspaces:

* **pydiods1** : a generic datasource that can be used to create workspaces shared accross users
* **personal** : a user-oriented datasource where the personal files of each users are stored
* **cells** : another user-oriented datasource used when users create empty cells with no specific folders in them.

The 2 last ones are in fact used in cunjonction with "Template Paths" nodes: these paths contain contextual information (like the user name) and are resolved at runtime to point to the correct location. 

The data is located by default under `/home/{pydio_user}/.config/pydio/cells/data` unless you have modified this path using the Advanced settings of the installer. You should find the corresponding folders on your server (/pydiods1/, /cells/, /personal/). If you want to feed a datasource with existing data, just copy it inside the folder, then hit "Re-Index DataSource" in the settings panel. This will detect the new data and make it available for creating workspaces.

### Managing workspaces

To create a workspace go to **Data Management > Workspaces**, then click on **+WORKSPACE** located on the right.

Then this menu will appear :

[!image-popup:2_getting_started/create_workspace.png]



1. **Root nodes** : Using the auto-complete field, pick one or more nodes ( = files or folders ) from the available datasources to compose your workspace contents. The **cells** and **my-files** are specific paths that can only be selected (you cannot browse their children) and will be resolved automatically at run-time. Note that you can add as many nodes as necessary.    
2. **Label** : This is the name of the workspace.
3. **Description** : You can put a description about what your workspace contains for example.
4. **Default Rights** : You can choose Default Reading/Writing rights for this workspace. This will be applied to the "Root Role" of the application, which means to all users (unless one of their roles override this behaviour).
5. **Alias** : You should generally leave this empty, a unique alias will be generated for this workspace at save time. Still, you can pick your own alias (make sure to use only alphanumerical characters and more generally URI-compatible) characters.

After its creation, you can still modify a workspace by going to **Data Management > Workspaces** and clicking on the workspace of your choice.

[!image-popup:2_getting_started/modify_workspace.png]

### Providing access rights to your workspace

Once a workspace is created, use the Users, Groups and Roles interfaces to eventually grant access to your users (see next [chapter](/en/docs/cells/v1/users-roles-and-groups)).