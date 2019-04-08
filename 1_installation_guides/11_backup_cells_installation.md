## Backup and restore

We explain here how to backup and restore your instance of Pydio Cells, so that you do not loose data nor configuration in case of a disaster such as hard-drive failure, power loss, bad upgrade process, etc.

This procedure is adapted for simple mono-node installation. If you have made multi-node installation, be aware that you must backup both storage and datasource for each one of your nodes.

### Backup configuration and default data location

If you have made a vanilla install, default _Home_ folder for the Pydio Cells installation is located at:

- `/home/pydio/.config/pydio/cells` for Linux users
- `~/Library/Application\ Support/Pydio/cells` for MacOS users

The `cells` folder contains all configuration. It is also the parent of the `data` subfolder that contains the default datasources that are created at install.

So create a backup of this folder, for instance:

```sh
cp -r ~/.config/pydio/cells cells-backup

# or with rsync
TODO put command
```

**Notes**:

- We advise you to use rsync to regularly backup this folder: after first time, the followings will be quicker because it only transfers the difference.
- If you ever happen to mess with your installation **without impacting the DB** you can always restore this folder to your last backup and then do a `./cells start` with a new or old binary. You should be good to go.
- All the information about network, database and plugins configuration are stored inside the `.../cells/pydio.json` file.

### Backup additionnal your datasources

All datasources are defined by 3 things:

- the **configuration** that is stored in the `pydio.json`file (see above)
- the **files** that are stored in this datasource 
-  

ya quoi dans le minio ? 
quid du versionoing ? de l'encription? 

To backup other datasources you only need to copy the parent repository of the datasource (this step is mandatory) because the parent repository contains the `.minio.sys` folder which is required to have a functioning datasource [*you can find more details about datasources here*](https://pydio.com/en/docs/developer-guide/data).

For instance your datasource is located in this path, `/home/pydio/datasource/firstds` the `.minio.sys` will be located in `/home/pydio/datasource/<location of .minio.sys>` then to back your datasource you must atleast back the parent folder `datasource` it's path being `/home/pydio/datasource` because it contains both your datasource(containing the data) and `.minio.sys`(the datasource config file).

### Backup your database

To back up the database you can use the `mysql dump` tool which will give you an sql file with all the datas at the moment of the backup, here's an example for the cells database, `mysqldump -u <user> -p cells > /home/pydio/cells_back.sql`,
to explain the command and the options, `-u <user>` is to use a specific mysql user, `-p` will prompt you for the mysql passowrd then `cells` is the database name, `> /home/pydio/cells.sql` is to choose where you want to target the save, you can choose which ever path you want as long as you can write inside it.

To import the database from your sql backup file use the following command : `mysql -u root -p cells < /home/pydio/cells_back.sql`,
in details, `-u <user>`, `-p` work as explained above, `cells`(make sure it already exists) is the database name then `< PathToTheSqlFile.sql`

### Recovery

As it is explained previously if you have saved all of your cells installation (the `~/.config/pydio/cells` folder) somewhere you can rest assured that it will always be usable, you only need a cells binary and the folder to be put in its default location which is for linux distributions `~/.config/pydio/cells` and for MacOS `~/Library/Application\ Support/Pydio/cells`, and then you can **start** cells `./cells start` if you copied all the folder it's initial location `~/.config/pydio/cells` (the path needs to be the same), cells will use the `pydio.json` to recover the configuration and will run directly you do not need to install it.

**If you recover on another server that has a different address you must change all of the occurences in the `pydio.json` such as bind_host, external_host and the datasources host.**

## Clean uninstallation

In this guide we will go through the steps to perform a clean uninstallation.

### How to perform a clean uninstall

On most of the OS pydio cells does not put the resources in multiple folders, it is concentrated in a single folder.
To remove cells you only have to remove the pydio cells folder and remove the cells database.

To remove the folder on _linux_ distributions you can make use of rm such as :
`rm -r ~/.config/pydio/cells`.

To remove the cells folder on _MacOS_ this :
`rm -r ~/Library/Application\ Support/Pydio/cells`.
_For macos users do not make the mistake to remove the 'Pydio' folder the sync app also stores configuration inside it_.

For any additional datasource that you are not going to make use of, you can just clear the parent folder.

Then clean the database with this command after you have logged in mysql :
`drop database cells;` (or any other name if you named it).

And you are good to go, there is no more traces of the previous install or of cells in general.
