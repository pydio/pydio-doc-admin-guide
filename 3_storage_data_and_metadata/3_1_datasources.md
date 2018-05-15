In this chapter, we will first guide you throught the process of creating a new datasource, either based on a file system or on a Amazon S3 Object repository.

### Create and configure a datasource

In Pydio Cells to create a datasource go to **Data Management > Storage**
then on the top right of the screen press **+DATASOURCE** now you will have to fill fields such as :


[!image-popup:2_getting_started/datasource_interface.png]

* **DataSource Identifier** : the name of your datasource how it's identified within Pydio internally.

* **Internal port** : Port used by the underlying Object Storage to expose its S3 api on http (9002 by default). 

* **Storage Type** : `Local File System` will point to a folder on the server where the Cells service is running, or `Remote Object Storage (S3)` will connect to a remote object storage supported Amazon S3 Rest API.

* **[Storage Parameters]**: See below.

* **Versioning Policy**: How files version history will be kept. Please read the next page for more info. Leaving this empty does not enable versioning on this datasource.

* **Encryption**: Whether data is encrypted at rest on this datasource. Warning, the additionnal security brought by encryption comes at the cost of a higher CPU load (encryption and decryption require computation) and also a higher risk of loosing data in the case the decryption key get lost. Please refer to the corresponding section of the Security chapter to learn how to configure this feature.


#### Filesystem Datasources

The most standard driver you can use is actually the File System datasource: it has the huge advantage of storing the files directly on a local or network file system, thus on the other way round, to read any existing data stored in a file system and empower it with Pydio Cells capabilities.

* **Peer Address** : "Peer" the storage is located: this is necessary for distributed setup where Pydio Cells may run on various machines. Active peers should be automatically detected and proposed in the list.
* **Path** is the path to the directory on the server's file system that will be then served as the root of this datasource.  
This folder name will be exposed as an Amazon S3 bucket, for this reason **it must comply with the Bucket naming rules**, which are the same as the DNS sub-domains naming rules: must be lower case, without spaces, technical names.  
**The path must be a absolute** on the chosen peer, which means it must be a path starting from the root of the chosen server.  
Once you have chosen a peer, note that the systems automatically discovers the available directories that are then presented in a popup list.  
* **Storage is MacOS** : if the Peer is running on MacOS, enable this to perform UTF8 names normalization, or you may see strange issues with names containing accented characters.

#### S3 Datasources

This connector will allow the use of a remote S3-compliant storage as datasource for Pydio Cells. Under the hood, an object service is started in Gateway mode using the Api KEY/SECRET you will provide.

[!image-popup:2_getting_started/datasource_interface_S3.png]

* **Bucket name** : the name of the bucket on the remote storage to store the data of Pydio.

* **S3 Api Key** : the API key (can be seen as a login) that identifies you.

* **S3 Api Secret** : the API secret (can be seen as a password) that completes the identification process giving you the ability to store your data inside this bucket.

* **Internal Path** : Additional Path appendended to the bucket name when querying the remote storage.

* **Custom Endpoint** : If the remote storage is Amazon S3, you can leave this empty. Otherwise, provide here the http/https URL to connect to the s3-compatible storage.