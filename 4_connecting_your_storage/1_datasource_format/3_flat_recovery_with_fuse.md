The flat format storage is highly recommended to get the best performance. Switching to an "object storage" provides an unprecedented level of flexibility and scalability, but it can be daunting to see UUIDs instead of the original filenames.

The ["snaphsot" feature](./flat-storage-best-performances) creates an index of the data for better long-term archiving. With that snapshot and the corresponding UUID files, Cells can easily recreate a datasource with the original files/folders tree.

But what if you simply want to explore the storage "offline", without a Cells instance running, or without wanting to recreate a brand new datasource just to find a file inside a backup? Meet Cells Fuse.

## Cells Fuse command line

Cells Fuse is a dedicated tool used to directly interact with a snapshot database to search for a given file, retrieve its content, or event mount the whole tree as a FUSE volume on the machine. *This feature is only supported on Linux* (MacOSX can use MacFUSE, but requires a manual build of the binary).

Let's read the manual for this command:

```
$> ./cells-fuse 

DESCRIPTION

  This tool allows the local mounting of a flat datasource, given a "snapshot" has been created in the storage. 
  It does NOT require a running Cells/Cells Enterprise instance nor MySQL connection.
  It currently supports local filesystem and S3-based datasources.

  Using cells-fuse can be useful in case of emergency to recover files from a flat datasource.

EXAMPLES 

  1. Mount Local datasource
  $ ./cells-fuse mount -t /tmp/datasource -s file:///var/cells/data/pydiods1/snapshot.db

  2. Mount Remote S3 datasource (note the s3s scheme)
  $ ./cells-fuse mount -t /tmp/datasource -s s3s://API_KEY:API_SECRET@s3.amazonaws.com/MyBucketName/snapshot.db

  3. Lookup for a file (without mounting the datasource)
  $ ./cells-fuse lookup --storage file:///var/cells/data/pydiods1/snapshot.db --name "*" --type file --base "/folder/path"

  4. Copy a specific file (without mounting the datasource) to a local location
  $ ./cells-fuse copy --storage file:///var/cells/data/pydiods1/snapshot.db --from "/folder/path/file.ext" --to "./file.ext"

  5. Display tool version
  $ ./cells-fuse version

Usage:
  ./cells-fuse [flags]
  ./cells-fuse [command]

Available Commands:
  copy        Copy the content of a specific file from storage to a local file
  help        Help about any command
  lookup      Look up for a file without mounting the whole storage, equivalent to shell 'find' command.
  mount       Mount whole datasource as a local filesystem using FUSE
  version     Print version information

Flags:
  -h, --help   help for ./cells-fuse

```

As you can see, the tool provides 3 main commands : 

 - **lookup**: find a file directly inside the tree, similar to a find + grep command. 
 - **copy**: given its path in the tree, finds the corresponding file inside the storage and copy it to your output
 - **mount**: mounts the whole tree as a local, readonly filesytem. From there you can copy any files or folders as you would with a standard FS.

### Download cells-fuse binary

Cells Fuse is compiled at the same time as Cells and follows the same versioning policy. It is downloadable directly in the release folder (Linux AMD64): 

[https://download.pydio.com/pub/cells/release/{CELLS LAST VERSION}/linux-amd64/cells-fuse](https://download.pydio.com/pub/cells/release/)

### Storage support

Currently, Cells Fuse supports Local FS and S3/S3-compatible storage. As long as it finds the snapshot file in the same folder/bucket as the data.