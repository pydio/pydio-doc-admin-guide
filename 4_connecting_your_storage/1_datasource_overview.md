### Concepts

In previous versions of Pydio, Workspaces were used to both define a way to access datasources and a way to manage ACLs on the data. Pydio Cells now provides a strong decoupling between technical storages and business logic:

- the **datasources** define the actual storage where the data lies
- the **workspaces** point to any point in the tree of any datasource and defines ACLs by adding or removing permissions and policies.
- the **cells** are then virtual workspaces that point to _many_ points in the various datasource trees and add share features and metadata.

Each **datasource** provides access to data internally and maintains its own consistent index by continuously listening to and storing changes. Indexes from the various datasources are then aggregated by the `pydio.grpc.tree` service into a global tree that is exposed to and used by other services. That way, datasources can be distributed across as many nodes as needed.

### How it works

When you upload data through the web-ui there is a specific task that is triggered, it will notify the **index** about the newly uploaded node, which will update its database(where he keeps track of every node) **(_see: [services involved](./services-involved)_)**,
once the files/folders are added in the index they are ready to be used.

### The importance of an update to date index

If you add/delete data directly on the storage (object storage, s3, filesystem) you will also have to manually resync (meaning, to update the [index](./services-involved)) to have it appear and be usable.

As long as you interact with the data through the Cells gateway with the following means:

- WebUI
- [Cells-client CLI](https://github.com/pydio/cells-client)

data will then be automatically indexed and ready to be used.

### Supported Storages

Datasource can be seen as a driver to access your data. It can currently be connected to two types of storages:

- **Local FileSystem**: folder located on the same server as where the service is running
- **Object Storage**: S3-compatible remote storage (can be a proper S3 or anything implementing the API).
- **Google Cloud Storage**: compatible with Google's cloud storage solution [ED only].
- **Azure Blob Storage**: compatible with Azure's blob storage solution [ED only].


[In next chapter](./creating-datasources), we show you how to create and manage datasources via the admin interface.

[Detailed structure of a datasource](./services-involved), in this article a detailed view of how each service on datasource interacts.