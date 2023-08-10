## Embracing the "objects storage" world

In flat storage, files are stored as basic data blobs named with their internal UUIDs. Folders are not physically present inside the storage. The index service maintains the whole tree of folders and files and will just query the storage when an actual data modification (upload/download) is necessary.
As a result, tree modifications like "moves" are performed directly inside the index, at best speed. 

[:image:4_connecting_your_storage/datasource-format-flat.png]

This is exactly what object storage technologies are designed for : serving objects by keys, allowing infinite horizontal scaling possibility.

## Backup / Restore

For admins, it can be a bit scary to have a look at their storage and see just a bunch of UUID-named files. What if Cells is down, how will I get my folders/files back? A command line tool is provided to back up a flat datasource index directly inside a dedicated DB file on the storage. This DB file can later on be read by Cells to initialize a new datasource with the saved index data.

To back up the index of "ds1" inside a "snapshot.db" file, run:
```
cells admin datasource snapshot --datasource=ds1 --operation=dump --basename=snapshot.db 
```
Then you can restore this index with the opposite command:
```
cells admin datasource snapshot --datasource=ds1 --operation=load --basename=snapshot.db 
```
Beware that the "load" operation will override the current index data! You can also create a new datasource and specify "snapshot.db" in the Advanced options.

_[Note]_ Performing regular backup of your SQL Database is enough to make sure that you are able to recover your files hierarchy. This optional on-file snapshot of the index table is a second layer of security, in case you lose your DB!

### Generating snapshots with Scheduler/CellsFlows

As an addition to the command-line, the "dump" operation can be manually triggered from the scheduler. For Enterprise Distribution, an additional job is programmed to run every night and dump snapshots for every Flat datasources. By default, these snapshots are kept for 10 days.

### Loading snapshot with Cells Fuse

The flat datasources snapshot can also be mounted and explored offline (without any running Pydio Cells) to recover one or more files. [Read more about Cells Fuse](./recovering-flat-storage-datasource-cells-fuse).