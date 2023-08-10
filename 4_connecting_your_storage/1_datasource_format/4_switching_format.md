## Command Line Tool

A command-line tool is provided to eventually migrate a datasource from flat to structured and opposite. 

```
cells admin datasource migrate
```

This tool is a command-line, as the server must run in a particular state to allow this migration. Basically, all `pydio.grpc.data.sync.*` services associated with each datasource must be **turned off** during a migration operation. When using the command line, it will check against the service registry that the targeted sync service is not running.

## Recommendations

It is better to not migrate data "in-place". For this reason, if a datasource "ds1" is stored in the bucket "bucket1" on the storage, you first have to create a new bucket "bucket2" where all data will be moved. Then, according to the new target format, the tool will move all files with correct renaming and eventually add or remove `.pydio` entries from the index. 

After a successfull operation, the tool will upgrade the configuration file to point the datasource to the new bucket. Files and folders entries in the index are always left untouched. After a restart, the operation should be transparent.

Please note that this tool can be handy in the case of full uninstallation of Pydio Cells. Use it to transform a flat storage to a structured one **before** uninstalling Cells!