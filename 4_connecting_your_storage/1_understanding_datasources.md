### Concepts

In previous versions of Pydio, Workspaces were used to both define a way to access datasources and a way to manage ACLs on the data. Pydio Cells now provides a strong decoupling between technical storages and business logic:

- the **datasources** define the actual storage where the data lies
- the **workspaces** point to any point in the tree of any datasource and defines ACLs by adding or removing permissions and policies.
- the **cells** are then virtual workspaces that point to _many_ points in the various datasource trees and add share features and metadata.

Each **datasource** provides access to data internally and maintains its own consistent index by continuously listening to and storing changes. Indexes from the various datasources are then aggregated by the `pydio.grpc.tree` service into a global tree that is exposed to and used by other services. That way, datasources can be distributed across as many nodes as needed.

### Services Involved

Each datasource is virtually composed 3 micro-services :

- An **Object service**: Provides access to the data, and can be accessed by any tool that talks Amazon S3 protocol. This object storage is defined by its URL and its Bucket name. This service is really in charge of talking to the underlying storage.
- An **Index service**: Stores the data state. It stands as the only source of truth for request on data state. This uses a highly optimized SQL model for storing/retrieving hierarchical data information (using nested sets).
- A **Synchronizer**: Receives events from the storage and maintains the index synchronized.

[:image-popup:4_connecting_your_storage/datasources.png]

The unidirectional synchronizer is key here:  
when a change is applied to the storage (e.g. a user uploads an image), the sync only receives an event _after this operation has finished_. It then updates the index accordingly. The same event is also forwarded to other services like the websocket service (for updating user interface), the scheduler (for computing image thumbnail), and so on.

One interesting point of design: when declaring a new data source with local file system storage type, e.g. on a folder **folder-name** on the server, the corresponding object service (a minio server) is started pointing to the **parent** folder. It exposes the folder **folder-name** as an S3 bucket.  
For this reason, if more than one datasource point to sibling folders, a factorization mechanism starts only _one_ Object storage on the parent folder and exposes all the children as s3 buckets.  
That's why we say that datasources are _virtually_ composed of 3 sub-services.

### Seeing it in action

Each services of the datasources are forked by the main `cells` process. For this reason, when you look at the running processes, you will see something similar to the ones below:

```sh
$ ps au | grep cells
pydio 85237  17,4  0,5 0:13.85 ./cells start
pydio 85239   0,9  0,3 0:01.15 ./cells start --fork pydio.grpc.data.objects.local1
pydio 85246   0,9  0,3 0:00.91 ./cells start --fork pydio.grpc.data.index.cells
pydio 85249   0,8  0,3 0:00.96 ./cells start --fork pydio.grpc.data.index.pydiods1
pydio 85241   0,8  0,3 0:01.35 ./cells start --fork pydio.grpc.data.sync.newdatasource
pydio 85240   0,7  0,3 0:01.28 ./cells start --fork pydio.grpc.data.sync.cells
pydio 85238   0,7  0,3 0:01.08 ./cells start --fork pydio.grpc.data.objects.local2
pydio 85243   0,7  0,3 0:01.26 ./cells start --fork pydio.grpc.data.sync.personal
pydio 85242   0,7  0,3 0:01.33 ./cells start --fork pydio.grpc.data.sync.pydiods1
pydio 85248   0,6  0,3 0:00.81 ./cells start --fork pydio.grpc.data.index.newdatasource
pydio 85247   0,6  0,3 0:00.85 ./cells start --fork pydio.grpc.data.index.personal
```

As you can see here, there are 2 objects services launched (local1, local2) and 4 datasources defined.

### Supported Storages

Datasource can be seen as a driver to access your data. It can currently be connected to two types of storages:

- **Local FileSystem**: folder located on the same server as where the service is running
- **Object Storage**: S3-compatible remote storage (can be a proper S3 or anything implementing the API).
- **Google Cloud Storage**: compatible with Google's cloud storage solution [ED only].
- **Azure Blob Storage**: compatible with Azure's blob storage solution [ED only].

[In next chapter](en/docs/cells/v2/managing-datasources), we show you how to create and manage datasources via the admin interface.
