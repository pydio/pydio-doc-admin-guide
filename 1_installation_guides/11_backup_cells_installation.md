## Create a backup of your Cells installation and recover from it

In this guide we are going to explain how you can create a backup of your Pydio Cells instance to make sure that if an issue occurs at any time being such as hard-drive failure, a power loss, bad upgrade process and so on, not data or configuration is lost.

We are also going to see how you can recover your cells installation assuming that you backed it up.

### Back up the configs & default Data Location

To create a backup of your pydio cells installation which is quite simple, you just need to copy the entirety of the pydio cells folder in another location(another hard-drive etc... wherever you want at your discretion).

For instance on my linux (debian) installation my cells is located here `~/.config/pydio/cells`
therefore i have to use this command `cp -r ~/.config/pydio/cells cells-backup`**(1)** to copy and therefore back my pydio cells.

_We advise you to regularly do a backup once it's done it will take lesser time to do the next copy to the same folder will just add the new data onto it_.

Now that the cells installation is backed up if you ever face an issue you can just copy back this folder into `~/.config/pydio/cells` then do a `./cells start` with a new or old binary and you are good to go.

All the informations about the host, the database, plugins configurations etc.. are stored inside this file `~/.config/pydio/cells/pydio.json`

You can also backup all of your database, one way could be to export the database into a sql file another would be to copy the database into another one.

__(1)__ For macos users the path is `~/Library/Application\ Support/Pydio/cells` the `~` being your home path.

### Back your datasources

To back other datasources you only need to copy the parent repository of the datasource (this step is mandatory) because the parent repository contains the `.minio.sys` folder which is required to have a functioning datasource [*you can find more details about datasources here*](https://pydio.com/en/docs/developer-guide/data).

For instance your datasource is located in this path, `/home/pydio/datasource/firstds` the `.minio.sys` will be located in `/home/pydio/datasource/<location of .minio.sys>` then to back your datasource you must atleast back the parent folder `datasource` it's path being `/home/pydio/datasource` because it contains both your datasource(containing the data) and `.minio.sys`(the datasource config file).

### Back up your database

To back up the database you can use the `mysql dump` tool which will give you an sql file with all the datas at the moment of the backup, here's an example for the cells database, `mysqldump -u <user> -p cells > /home/pydio/cells_back.sql`,
to explain the command and the options, `-u <user>` is to use a specific user, `-p` will prompt you for the mysql passowrd then `cells` is the database name, `> /home/pydio/cells.sql` is to choose where you want to target the save, you can choose which ever path you want as long as you can write inside it.

To import the database from your sql backup file use the following command : `mysql -u root -p cells < /home/pydio/cells_back.sql`,
in details, `-u <user>`, `-p` work as explained above, `cells`(make sure it already exists) is the database name then `< PathToTheSqlFile.sql`

### Recovery

As it is explained previously if you have saved all of your cells installation (the `~/.config/pydio/cells` folder) somewhere you can rest assured that it will always be usable, you only need a cells binary and the folder to be put in its default location which is for linux distributions `~/.config/pydio/cells` and for MacOS `~/Library/Application\ Support/Pydio/cells`, and then you can **start** cells `./cells start` if you copied all the folder it's initial location `~/.config/pydio/cells` (the path needs to be the same), cells will use the `pydio.json` to recover the configuration and will run directly you do not need to install it.

**If you recover on another server that has a different ip you must change all of the occurences in the `pydio.json` such as bind_host, external_host and the datasources host.**