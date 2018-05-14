### Concepts

In previous versions of Pydio, Workspaces were used to both define a way to access datasources and a way to manage ACLs on the data. Pydio Cells now provides a strong decoupling between technical storages and business logic: "datasources" define actual storages where data lies, and workspaces can point to any point of the datasources trees to define ACLs. This is in fact how the Cells feature was achieved.

Technically speaking, a datasource provides access to data internally, by continuously listening to and storing changes, to maintain its own consistent data index. These indexes are then aggregated by Pydio into a global tree (by the pydio.grpc.tree service) and used by the other services. That way, datasources can be distributed across many nodes as needed. 

### Services Involved

Each datasource is virtually composed 3 micro-services :

* **An Object service** : Provides access to the data, and can be accessed by any tool that talks Amazon S3 protocol. This object storage is defined by its URL and its Bucket name. This service is really in charge of talking to the underlying storage.
* **An Index service** : Stores the data state and stands as the only source of truth for request on data state. This uses a highly optimized SQL model for storing/retrieving hierarchical data information (using nested sets).
* **A Synchronizer** : Receives events from the storage and maintains the index synchronized 

The unidirectional synchronizer is the key here : when any change is applied to the storage (e.g. a user uploads an image), the sync will receive an event only once this operation is finished, and then update the index accordingly. This event will also be forwarded to other services like the websocket service (for updating user interface), the scheduler (for computing image thumbnail), and so on.

By design, when defining Object storage e.g. on a folder **folder-name** on the server, it will start on the **parent** folder and expose the folder **folder-name** as an S3 bucket. For this reason, if many datasources point to sibling folders, a factorization mechanism will start only one Object storage on the parent folder and expose all the children as s3 buckets. That's why we say that datasources are "virtually" composed of 3 sub-services. 

### Seeing it in action

Each services of the datasources are forked by the main cells process. For this reason, when you look at the running processes, you will see something similar to the ones below:

```
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

Datasource can be seen as a driver to access your data. It can currently be connected to two types of storages : 

* **Local FileSystem** : folder located on the same server as where the service is running
* **Object Storage**: S3-compatible remote storage (can be a proper S3 or anything implementing the API).

You will see next how to create and manage datasources via the admin interface.