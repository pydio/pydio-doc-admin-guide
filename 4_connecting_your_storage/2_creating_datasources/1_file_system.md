The most standard driver you can use is actually the **File System datasource**: it has the huge advantage of storing the files directly on a local or network file system, thus on the other way round, to read any existing data stored in a file system and empower it with Pydio Cells capabilities.

[:image:4_connecting_your_storage/datasource_config/file_system_interface.png]

- **Peer Address**: address of the server where the storage is located. This is necessary for distributed setup where Pydio Cells may run on various machines. Active peers should be automatically detected and proposed in the list. (You can now choose the hostname which in case of an ip change will not disturb your datasource)
- **Path**: path to the directory on the server file system that is served as the root of this datasource.
- **Storage is MacOS**: if the Peer is running on MacOS, enable this to perform UTF8 names normalization, or you may see strange issues with names containing accented characters.

**Warning** when defining the path to the root folder of your datasource on the server, observe the following:

*The path must be at least 2 levels down, for instance `/mnt/data/`*

- **The path must be absolute** on the chosen peer: it must start from the root of the chosen server.  
  Once you have chosen a peer, note that the system automatically discovers the available directories and presents them in a popup list.
- Due to the internal implementation of the file-system-based Object service (see [previous chapter](./datasources-overview)):
  - The leaf folder is exposed as an Amazon S3 bucket, **it must comply with the Bucket naming rules**, which are the same as the DNS sub-domains naming rules: technical names that must be lower case and without spaces.
  - The path must be at least two levels deep (defining e.g. `/tmp` as a datasource does not work).
  - The user owning the process that starts the app (usually the `pydio` user) must have read and write permissions on this folder **and on the parent folder**.

If you wish to create a folder during that step, select the parent from the list, then you will see the "new" button appear on the line.

[:image:4_connecting_your_storage/datasource_config/file_system_create_folder.png]

### Flat Format data distribution

When creating a Flat datasource inside an FS storage, you will end up storing all the files at the root of the picked folder (as they are just referenced by their IDs). If you underlying storage technology does not like this kind of layout, you can use the automatic sharding feature by spreading files inside automatic folders and subfolders, based on their IDs. 

The parameter can be defined using the following pattern: `N:P:Q...` where N, P, Q are integers and represent levels of folders where folders are named from the file UUID's first characters. For example, using '1:2' for sharding would create a first level of folders with the first letter, and inside a second level of folders with the two first letters of the uuid. Using random UUIDs, this would look like:

 - a/ab/abzraat2R2E
 - a/ad/ad2ererzer
 - q/q2/q2zzt43gezzet
 - etc...