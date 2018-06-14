In this chapter, we guide you throught the process of creating a new datasource, either based on a file system or on a Amazon S3 Object repository.

### Create and configure a datasource

In Pydio Cells to create a datasource go to **Data Management > Storage**
then on the top right of the screen press **+DATASOURCE** now you will have to fill fields such as :

[:image-popup:2_getting_started/datasource_interface.png]

- **DataSource Identifier**: Internal id of this datasource.
- **Internal port**: Port used by the underlying Object Storage to expose its S3 api on http (9002 by default).
- **Storage Type**:
  - `Local File System` enable management of files that are effectively stored in a file system on the chosen machine (might be the one where the Cells service is running or another one),
  - `Remote Object Storage (S3)` connects to any remote object storage complying to the Amazon S3 API and exposes its objects as a tree.
- **[Storage Parameters]**: See below.
- **Versioning Policy**: How files version history will be kept. Please read the next page for more info. Leaving this empty disable versioning for this datasource.
- **Encryption**: Whether data is encrypted at rest on this datasource. Warning, the additionnal security brought by encryption comes at the cost of a higher CPU load (encryption and decryption require computation) and also a higher risk of loosing data if the decryption key gets lost. Please refer to [the corresponding section](/en/docs/cells/v1/using-encryption) of the Security chapter to learn how to configure this feature.

#### Filesystem Datasources

The most standard driver you can use is actually the File System datasource: it has the huge advantage of storing the files directly on a local or network file system, thus on the other way round, to read any existing data stored in a file system and empower it with Pydio Cells capabilities.

- **Peer Address**: address of the server where the storage is located. This is necessary for distributed setup where Pydio Cells may run on various machines. Active peers should be automatically detected and proposed in the list.
- **Path**: path to the directory on the server file system that is served as the root of this datasource.
- **Storage is MacOS**: if the Peer is running on MacOS, enable this to perform UTF8 names normalization, or you may see strange issues with names containing accented characters.

**Warning** when defining the path to the root folder of your datasource on the server, observe the following:

- **The path must be absolute** on the chosen peer: it must start from the root of the chosen server.  
  Once you have chosen a peer, note that the system automatically discovers the available directories and presents them in a popup list.
- Due to the internal implementation of the file-system-based Object service (see [previous chapter](/en/docs/cells/v1/understanding-datasources)):
  - The leaf folder is exposed as an Amazon S3 bucket, **it must comply with the Bucket naming rules**, which are the same as the DNS sub-domains naming rules: technical names that must be lower case and without spaces.
  - The path must be at least two levels deep (defining e.g. `/tmp` as a datasource does not work).
  - The user owning the process that starts the app (usually the `pydio` user) must have read and write permissions on this folder **and on the parent folder**.
  
#### S3 Datasources

This connector allows using a remote S3-compliant storage as datasource for Pydio Cells. Under the hood, an object service is started in Gateway mode using the Api KEY/SECRET you will provide.

[:image-popup:2_getting_started/datasource_interface_S3.png]

- **Bucket name**: the name of the bucket on the remote storage to store the data of Pydio.
- **S3 Api Key**: the API key (can be seen as a login) that identifies you.
- **S3 Api Secret**: the API secret (can be seen as a password) that completes the identification process giving you the ability to store your data inside this bucket.
- **Internal Path**: Additional Path appendended to the bucket name when querying the remote storage.
- **Custom Endpoint**: If the remote storage is Amazon S3, you can leave this empty. Otherwise, provide here the http/https URL to connect to the s3-compatible storage.
