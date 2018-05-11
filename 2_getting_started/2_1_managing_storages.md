
Storage management in **Pydio Cells** has completely changed compared to the previous versions. In this chapter, we will present the workflow used in details and explain how you can manage your storage.

## Storage

With Pydio Cells, you create a datasource that can be seen as a driver to access your data. When you define a new workspace, you then choose among the specified data source which one to use.

A deeper dive in the datasource / workspace concepts is done in the next chapter. Here, we will introduce the minimum information necessary for you to start using Cells with a minimum of effort.

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

![datasource interface](/images/2_getting_started/datasource_interface.png)

* **DataSource Identifier** : the name of your datasource how it's identified within Pydio.

* **Internal port** : By default the port is 9002.

* **Storage** : For the moment you have 2 options, `Local File System` using your local storage or `Remote Object Storage (S3)` to connect to a remote object storage as it's named.

For the `Local file System` :

![datasource interface local file system](/images/2_getting_started/datasource_interface_local_file_system.png)


* **Peer Address** : Where the storage is located.

* **Local folder** : the path to the folder where the data will be stored.

* **Storage is MacOS** : if your on MacOS this has to be `enabled`.

* **Versioning Policy** : you can choose how you want to handle the versioning for your storage.

* **Use Encryption** : You can cypher the data stored in this storage, you need an encryption key ( you must not loose it, if you want to backup data it will be lost )
be wary that it's really heavy on the CPU so for a smooth experience have plenty of resources.


The `Remote Object Storage (S3)` requires different fields such as :

![datasource interface s3](/images/2_getting_started/datasource_interface_S3.png)

* **Bucket name** : the name of the bucket created to store the data of Pydio.

* **S3 Api Key** : the key (can be seen as a login) that identifies you.

* **S3 Api Secret** : the secret (can be seen as a password) that completes the identification process giving you the ability to store your data in your bucket.

* **Internal Path** : how the data should be structured inside the bucket.

* **Custom Endpoint** : your access policy to the storage.
