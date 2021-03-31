We explain here how to backup and restore your instance of Pydio Cells, so that you do not loose data nor configuration in case of a disaster such as hard-drive failure, power loss, bad upgrade process, etc.

This procedure is adapted for simple mono-node installation. If you have made multi-node installation, be aware that you must backup both storage and datasource for each one of your nodes.

### Backup configuration and default data location

If you have made a vanilla install, default `CELLS_WORKING_DIR` folder for the Pydio Cells installation is:

- `/home/pydio/.config/pydio/cells` for Linux users
- `~/Library/Application\ Support/Pydio/cells` for MacOS users

The `cells` folder contains all configuration.  
The `cells` folder is also the parent of the `data` subfolder that contains the default datasources that are created at install. Please note that these default datasources contains in addition to the default _business_ datasources, the hidden internal datasource that is used to store among others version files, thumbnails, etc.

So create a backup of this folder, for instance:

```sh
# If possible, it is always to stop Cells before doing the backup
systemctl stop cells

# Using rsync, first time will be longer and then we will only incrementally add and/or remove new files
rsync -avr --delete /home/pydio/.config/pydio/cells/ /home/pydio/backups/cells
```

**WARNING**: proceed with extra care with rsync when using the `--delete` flag:  
inverting source and target folders will wipe everything in the source folders...  
So be careful to really use: `rsync -avr --delete <source folder> <target folder>`

**Notes**:

- We advise you to use rsync to regularly backup this folder: after first time, the followings will be quicker because it only transfers the difference.
- If you ever happen to mess with your installation **without impacting the DB** you can always restore this folder to your latest backup and then do a `./cells start` with a new (or a restored old) binary. You should be good to go.
- All the information about network, database and plugins configuration are stored inside the `<CELLS_WORKING_DIR>/pydio.json` file.

### Backup additionnal datasources

#### File system datasource

All _file based_  datasources are defined by 4 things:

1. the **configuration** that is stored in the `pydio.json`file (see above)
1. the **files** that are stored in this datasource
1. a related index that is stored as tables in the configured DB
1. _for file system based DS_: a `.minio.sys` folder that is located in the parent of the DS root folder (this contains s3/minio meta data for the files)

You can find [more details about datasources here](../../developer/data).

To backup such a datasource, we must at least backup configuration (see preceding paragraph) and the files.
The index and the content of the `.minio.sys` folder can be automatically restored after data restoration by running a resync from the admin interface.
You can yet safely include this folder in your backups: you will gain quite some time on restoration if you do not have to re-compute the index.

#### S3 datasource

This is one of the main advantage of using a S3 backend for your datasources: this system is fully decoupled from your Pydio Cells instance.
You should be able to directly manage your backup policies via your S3 provider manager interface.

The configuration of this datasource is fully stored in the main `pydio.json` file and is backed up and restored together with your main system.

Note that the Enterprise distribution offers two more datasource types (namely Google Cloud and Azure Blob storage) that are to be backed up the same way.

### Backup the database

In a vanilla single node instance, at installation, you have configured a database connection, usually the `cells` DB. In such case, you only need to backup (and restore) this single database.

To perform a backup, you can use default mysql tool that is usually installed with your DB software.  
Typically, on Linux:

```sh
mysqldump -u pydio -p cells > /home/pydio/backups/cells_sqldump_$(date -u +%Y-%m-%d).sql
```

where:

- `-u <user>`: defines the user to be used
- `-p` prompts you for the mysql password
- `cells`: your database name
- `> /home/pydio/backups/cells_sqldump_$(date -u +%Y-%m-%d).sql`tells the tool where to store resulting data.

You might adapt these options to your use case.

### Recovery

#### Restoring the data

If you have followed the above steps, restoring the data is quite easy:

- Restore the content of the `cells` folder (under `CELLS_WORKING_DIR`, e.g. `/home/pydio/.config/pydio/cells` on Linux or `~/Library/Application\ Support/Pydio/cells` on MacOS)
- Restore the database from your sql backup file: make sure to create an empty `cells` with correct owner and use the following command:
  `mysql -u pydio -p cells < /home/pydio/backups/cells_sqldump_<relevant_date>.sql`
- Optionally restore the folders of any additional external filesystem datasources

#### Post restoration and before launching the app

If you restore the application on a different server or if some of the configuration like database, URLs, IP addresses have changed, you must double check in the `pydio.json` configuration file that can be found at the root of the `CELLS_WORKING_DIR` folder and adapt the values to the new one, typically:

- if hostname as changed, change `.defaults.urlInternal` property, you might also want to check `.PeerAdress` property of the various DS
- if public URL has changed, you have to change all occurrences of it (currently 4 in v1.4 and newer)
- if DB configuration has changed: adapt `.databases.dsn` property

You can then retrieve the relevant Pydio Cells binary and simply relaunch the app.

You should connect as an `admin` user after first restart and check that all datasources are correctly up and running, and optionally also rerun a sync if you where not able to correctly backup and restore the index **and** parent `.minio.sys` folder.

## Clean uninstall

If you want to uninstall Cells, you need to :

- Stop cells service
- Drop the database cells
- Empty the [Working Directories](./working-directories)

_**WARNING**: be careful not to remove the parent 'pydio' folder, other applications, typically the Cells Sync Client, have their working default directory as siblings of the `cells` folder and store configuration and data inside it_.
