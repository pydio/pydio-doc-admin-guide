## Backup and restore

We explain here how to backup and restore your instance of Pydio Cells, so that you do not loose data nor configuration in case of a disaster such as hard-drive failure, power loss, bad upgrade process, etc.

This procedure is adapted for simple mono-node installation. If you have made multi-node installation, be aware that you must backup both storage and datasource for each one of your nodes.

### Backup configuration and default data location

If you have made a vanilla install, default _Home_ folder for the Pydio Cells installation is located at:

- `/home/pydio/.config/pydio/cells` for Linux users
- `~/Library/Application\ Support/Pydio/cells` for MacOS users

The `cells` folder contains all configuration.  
The `cells` folder is also the parent of the `data` subfolder that contains the default datasources that are created at install. Please note that these default datasources contains in addition to the default _business_ datasources, the hidden internal datasource that is used to store among others version files, thumbnails, etc.

So create a backup of this folder, for instance:

```sh
cp -r ~/.config/pydio/cells /home/pydio/backups/cells-backup

# or with rsync
TODO put command
```

**Notes**:

- We advise you to use rsync to regularly backup this folder: after first time, the followings will be quicker because it only transfers the difference.
- If you ever happen to mess with your installation **without impacting the DB** you can always restore this folder to your latest backup and then do a `./cells start` with a new (or a restored old) binary. You should be good to go.
- All the information about network, database and plugins configuration are stored inside the `.../cells/pydio.json` file.

### Backup additionnal datasources

All datasources are defined by 4 things:

1. the **configuration** that is stored in the `pydio.json`file (see above)
1. the **files** that are stored in this datasource
1. a related index that is stored as tables in the configured DB
1. _for file system based DS_: a `.minio.sys` folder that is located in the parent of the DS root folder

You can find [more details about datasources here](https://pydio.com/en/docs/developer-guide/data).

To backup such a datasource, we must at least backup configuration (see preceeding paragraph) and the files.
The index and the content of the `.minio.sys` folder can be automaticcally restored after data restoration by running a resync from the admin interface.
You can yet safely include this folder in your backups.

### Backup the database

In a vanilla single node instance, you configure a database connection at install time, usually the `cells` db. In such case, you only need to backup (and restore) this single database.

To perform a backup, you can use default mysql tool that is usually installed with your DB softaware.  
Typically on Linux:

```sh
mysqldump -u pydio -p cells > /home/pydio/backups/cells_sqldump_$(date -u +%Y-%m-%d).sql
```

where:

- `-u <user>`: defines the user to be used
- `-p` prompts you for the mysql passowrd
- `cells`: your database name
- `> /home/pydio/backups/cells_sqldump_$(date -u +%Y-%m-%d).sql`tells the tool where to store resulting data.

You might adapt these options to your use case.

### Recovery

#### Restoring the data

If you followed the above step, restoring the data is quite easy:

- Restore the `cells` parent folder (under e.g. `/home/pydio/.config/pydio/cells` on Linux or `~/Library/Application\ Support/Pydio/cells` on MacOS)
- Restore the database from your sql backup file: make sure to create an empty `cells` with correct owner and use the following command:
  `mysql -u pydio -p cells < /home/pydio/backups/cells_sqldump_<relevant_date>.sql`
- Optionnaly restore the folders of any additional external filesystem datasources

#### Post restoration and before launching the app

If you recover on another server or if some of the configuration like database, URLs, IP addresses have changed, you must double check in the `pydio.json` configuration file that can be found at the root of the `cells` folder and adapt the values to the new one, typically:

- if hostname as changed, change `.defaults.urlInternal` property, you miight also want to check `.PeerAdress` property of the various DS
- if public URL has changed, you have to change all occurences of it (currently 4 in v1.4 and newer)
- if DB configuration has changed: adapt `.databases.dsn` property

You can then retrieve the relevant Pydio Cells binary and simply relaunch the app.

You should connect as an `admin` user after first restart and check that all datasources are correctly up and running, and optionnaly also rerun a sync if you where not able to correctly backup and restore the index and parent `.minio.sys` folder.

## Clean uninstallation

In vanilla single node setups, Pydio Cells does not put resources in multiple folders: everything is centralized in the `cells` folder and in the database.
So, to remove Pydio Cells, you mainly need to remove these 2 items and the Cells binary.

To remove the folder on _linux_ distributions you can make use of rm such as :
`rm -r ~/.config/pydio/cells`.

To remove the cells folder on _MacOS_ this :
`rm -r ~/Library/Application\ Support/Pydio/cells`.
_For macos users do not make the mistake to remove the 'Pydio' folder the sync app also stores configuration inside it_.

For any additional datasource that you will not use anymore, you can clear the parent folder.

Then clean the database with this command after you have logged in mysql :
`drop database cells;` (or any other name if you named it).

You are then good to go: there is nothing else remaining of the previous install or of Pydio Cells in general.
