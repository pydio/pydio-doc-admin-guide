In structured format, the files and folders you see in Cells are exactly the same as the storage contents.

## Objects-to-Index Synchronization

Any modification, even going through the Cells interface is always applied on the storage, and then reported to the index throught the real-time synchronization. 

[:image:4_connecting_your_storage/datasource-format-struct.png]

In this configuration, modifying the storage content directly and triggering a manual re-synchronization makes sure that the index is always up-to-date.

## Drawbacks

As Cells is speaking "object" with the storage, the drawback of this configuration is that huge moves (like a folder rename) require first moving all the objects inside the folder to their new name, listening to these move events, factorizing them and finally renaming the folder inside the index. Furthermore, the "move" operation does not really exist in object storages, but always require a copy/delete sequence!

Another drawback is the presence of ".pydio" hidden files inserted in each folder to maintain their UUID. 

## Backup / Restore

For structured storage, backup/restore is just a matter of copying the data to/from an other location and launching a resync manually.

