### Concepts

Pydio Cells provides a strong decoupling between technical storages and business logic:

- **Datasources** define the actual storage where the data lies. They can be seen as "mount points" for external storages. Each datasource provides access to data internally and maintains its own consistent index in the database. Indexes from the various datasources are aggregated into a global tree that is exposed to other services. That way, datasources can be distributed across as many nodes as needed.

- **Workspaces** are defined by administrators. Admins open accesses to one or many node in the global tree of files inside datasources by adding or removing permissions and policies on these nodes.

- **Cells** are sandboxed workspaces created by users to share data between themselves. Users easily manage accesses while still inheriting the restrictions set by administors.

### Supported Storages

Datasource can be seen as a driver to access your data. It can currently be connected to the following storage types:

- **Local FileSystem**: folder located on the same server as where the service is running
- **Object Storage**: S3-compatible remote storage (can be a proper S3 or anything implementing the API).
- **Google Cloud Storage**: compatible with Google's cloud storage solution [ only].
- **Azure Blob Storage**: compatible with Azure's blob storage solution [ only].

In the article [Detailed structure of a datasource](./services-involved), a detailed view of how each service on datasource interacts.

### Flat vs. Structured Storage Format 

Starting with Cells v3, the default storage format is "flat": while indexes maintain the tree of files and folders in the database, actual data is stored as blobs on the storage. Using this format, you are not supposed to modify the storage directly (without going through Cells). 

If you need to be able to modify data directly on the storage, you will have to use "structured" storage format, and manually trigger a resynchronization to see the modifications appear and be usable. This operation can be done with a REST api call or with the CLI. Interacting with the data via the standard clients (Cells Web interface, [Cells-client CLI](https://github.com/pydio/cells-client) or CellsSync) you should never care about re-indexation.

Read more about the [datasource format](./datasource-format) in the next chapter.

