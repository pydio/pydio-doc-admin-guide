### Concepts

Pydio Cells provides a strong decoupling between technical storages and business logic:

- **Datasources** define the actual storage where the data lies. They can be seen as "mount points" for external storages. Each datasource provides access to data internally and maintains its own consistent index in the database. Indexes from the various datasources are aggregated into a global tree that is exposed to other services. That way, datasources can be distributed across as many nodes as needed.

- **Workspaces** are defined by administrators and to open accesses to one or many node in the global tree of files inside datasources, by adding or removing permissions and policies on these nodes.

- **Cells** are sandboxed workspaces created by users to share data between themselves. Users easily manage accesses while still inheriting the restrictions set by administors.


### How it works

When you upload data through the application there is a specific task that is triggered on a micro-service, which will consequently notify the **index** about the newly uploaded node, who will update its database (where he keeps track of every node) **(_see: [services involved](./services-involved)_)**. Once the files/folders are added in the index they are ready to be used.

The index is the only source of truth on a datasource, which means for instance that if you have a node on your filesystem that it is not registered in the index, you will then not be able to see it on the application.

### The importance of an up-to-date index

If you add/delete data directly on the storage (object storage, s3, filesystem) you will also have to manually trigger the resync (meaning, updating the [index](./services-involved)) to have it appear and be usable. This operation can be done with a REST api call or with the CLI.

Interacting with the data via the standard clients (Cells Web interface, [Cells-client CLI](https://github.com/pydio/cells-client) or CellsSync) you should not need to care about re-indexation.

### Supported Storages

Datasource can be seen as a driver to access your data. It can currently be connected to the following storage types:

- **Local FileSystem**: folder located on the same server as where the service is running
- **Object Storage**: S3-compatible remote storage (can be a proper S3 or anything implementing the API).
- **Google Cloud Storage**: compatible with Google's cloud storage solution [ED only].
- **Azure Blob Storage**: compatible with Azure's blob storage solution [ED only].

In the article [Detailed structure of a datasource](./services-involved), a detailed view of how each service on datasource interacts.