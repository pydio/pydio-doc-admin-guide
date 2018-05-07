## Managing storage with Pydio Cells

Storage in Pydio cells has changed, the workflow is not the same as the previous versions we will see in details how you can manage your storage with Cells.

## Storage
In Pydio Cells storage has completely changed. In comparison, with Pydio 8, you had to mount the storage to your server to store data and then create a workspace using the path to that storage.

Now with Pydio Cells the workflow has changed. You create a datasource that will be used to store the data and then you can create your workspaces etc...

### Datasources

#### What is a Datasource ?
Datasources are the core of storage in Pydio, datasources are concrete storage locations that are aggregated by Pydio into a global tree. They can be distributed across many storage nodes as needed.

Technically speaking, a datasource provides access to data, it continuously listens and stores changes to maintain a consistent data index.

A datasource is composed of :

* **A Storage service** : Provides access to the data and can be accessed by any tool that talks Amazon S3 protocol.
* **An index service** : Stores the data state and stands as the only source of truth for request on data state.
* **A synchronizer** : Maintains the storage and the index database synchronized.

Every time a datasource has its state updated, the index service publishes events to notify the other services.

#### Using Datasources

In Pydio Cells to create a datasource go to ** Data Management > Storage **
then on the top right of the screen press **+DATASOURCE** now you will have to fill fields such as :

* **DataSource Identifier** : the name of your datasource how it's identified within Pydio.

* **Internal port** : By default the port is 9002.

* **Storage** : For the moment you have 2 options, `Local File System` using your local storage or `Remote Object Storage (S3)` to connect to a remote object storage as it's named.

For the `Local file System` :

* **Peer Address** : Where the storage is located.

* **Local folder** : the path to the folder where the data will be stored.

* **Storage is MacOS** : if your on MacOS this has to be `enabled`.

* **Versioning Policy** : you can choose how you want to handle the versioning for your storage.

* **Use Encryption** : You can cypher the data stored in this storage, you need an encryption key ( you must not loose it, if you want to backup data it will be lost )
be wary that it's really heavy on the CPU so for a smooth experience have plenty of resources.


The `Remote Object Storage (S3)` requires different fields such as :

* **Bucket name** : the name of the bucket created to store the data of Pydio.

* **S3 Api Key** : the key (can be seen as a login) that identifies you.

* **S3 Api Secret** : the secret (can be seen as a password) that completes the identification process giving you the ability to store your data in your bucket.

* **Internal Path** : how the data should be structured inside the bucket.

* **Custom Endpoint** : your access policy to the storage.

### Workspaces

Pydio Cells workspaces work the same way they used to in Pydio 8. The only changes are how drivers are managed. Because we are using **Datasources**, you can allocate a single or multiple nodes, to a workspace.

To create a workspace go to ** Data Management > Workspaces ** then click on the top right button **+WORKSPACE** then :

##### Driver options :
* **Root Nodes** : you can allocate one or many nodes to your workspace.

##### Set Generic Options :
* **Label** : name your workspace.
* **Description** : a brief Description of your workspace.
* **Default rights** : the default rights for users in this workspace.
* **Alias** : instead of the generic unique ID you can use an Alias.


## Sharing with Pydio Cells

### Cells

Cells is the new way of sharing data with Pydio Cells. You can now easily share and manage data with multiple users/groups. A cell is a new space that enables you to share, attach one or multiple folders to it and expand it as much as you want.

### Using Cells

There are two ways to create cells.

The main way is by going to your **HOME** page and finding the **Cells** menu on the left sidebar. Click on the **+** icon to have a new menu opening :

#### Create a Cell :

* **Title** : the name for the cell.
* **Description** : you can add a short Description of your cell.

Press **Next**

* **Users/groups** : You can now choose a single or multiple groups/users that will have access to this cell. You can also choose how they can interact within it.

* **Add an existing Folder** : You can either choose an existing folder and the cell will
be created within this folder or let Pydio create an empty folder for you.

**Go** now your cell is created.

Within a cell you can use the same features as if you where using a workspace such as compressing files, etc ...


#### Another way to create cells :

You can right click on any folder and choose **share** now you will be able to either add this folder to an existing cell or create a new cell with this new folder.
