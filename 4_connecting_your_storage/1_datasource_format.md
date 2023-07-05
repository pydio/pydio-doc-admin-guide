One requirement of many Pydio/PydioCells users was always to "keep the tree structure" of files and folders visible inside the storage (and eventually modify these directly without going through Pydio). To achieve this, Cells was conceived so that datasources rely on a unidirectionnal synchronization between the storage and the internal index. That way, the index is always reflecting the state of the storage, and if one wants to modify the storage content directly, a re-indexation is required.

But when files number is getting high, this can bring a bunch of issues, and sometimes a "resync" of the datasource is required to fix index issues.

Starting with Cells v3, a new datasource format was introduced, that keeps the tree structure only in Cells internal indexes, and stores files as a **flat structure** inside the storage. This is more inline with "object storage" design, and brings huge performance gains as no more "sync" is required to maintain these indexes. This is now the preferred format, unless a direct access to the files inside the storage is necessary.

This "format" is decoupled from the storage technology : any datasource (Local FS, S3 or S3 compatible, etc...) supports both formats.

[:summary]


### Special Case : "Internal" Storage

A third format, derived from Flat, is available to create specific datasources used for internal data, namely thumbnails and files versions. These datasources are using the same format as Flat, but have the specificity of not sending events to various parts of the application.
