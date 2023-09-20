Using any filesystem mounted as a volume should be working out-of-the-box similarly to a standard file system (see previous section). That said, you must take special care of the mounted volume structure and permissions.

### Storage layout - Internal folder

When using a local path as storage for a datasource, Cells will expose the given folder as an "s3 bucket". This implies storing some specific metadata inside a hidden folder located **at the upper level** of the target folder. 

For example, if a datasource is created on `/data/storage/datasource1`, Cells will expect to create its internal folder at `/data/storage/.minio.sys/`. When uploading a file as multipart, the parts go inside this folder, and the final file is rebuilt and moved at the correct location.

For this reason when creating this datasource, we must make sure that its parent folder `/data/storage` has the same permissions (pydio user), **and** is located on the same volume as the `datasource1` folder.

### Mounting a NAS / Remote storage

Any POSIX-compliant volume should be supported as a local FS by Cells. Typically, mounting a remote storage using the `cifs` or `nfs` protocols is very frequent. 

As explained above, in such case you must just make sure that **your target storage folder and its parent folder** are located in the same physical volume. To achieve this, the simplest way is to mount the parent instead of mounting the target folder directly. 

```
EXAMPLE : Remote storage exposes the following folder: /shares/data/datasource

WRONG
 - Mount /shares/data/datasource => Local /volumes/datasource
 - Creating a datasource on /volumes/datasource will fail as /volumes and /volumes/datasource are on differing volumes

CORRECT
 - Mount /shares/data => Local /volumes/data
 - Creating a datasource on /volumes/data/datasource will work
```

### Forcing Cross-Volume

Sometimes you don't have a choice, and you must be able to mount a remote storage with the "wrong" setup. Although it should never be used in production, there is a trick to force such configuration, by using the `CELLS_MINIO_ALLOW_CROSSMOUNT=true` environment variable.

Typically, that can be used to temporarily connect with the wrong setup and import all data at once, then using a clean datasource for actual usage.

**Note that this setup is dangerous and should not be used in production, as the cross-volume copy of uploaded files may be corrupted.**
