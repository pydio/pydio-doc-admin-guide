//TODO

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
