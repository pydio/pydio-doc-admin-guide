## Create a backup of your Cells installation and recover from it

In this guide we are going to explain how you can create a backup of your Pydio Cells instance to make sure that if an issue occurs at any time being such as hard-drive failure, a power loss, bad upgrade process and so on, not data or configuration is lost.

We are also going to see how you can recover your cells installation assuming that you backed it up.

### Back up everything

To create a backup of your pydio cells installation which is quite simple, you just need to copy the entirety of the pydio cells folder in another location(another hard-drive etc... wherever you want at your discretion).

For instance on my linux (debian) installation my cells is located here `~/.config/pydio/cells`
therefore i have to use this command `cp -r ~/.config/pydio/cells cells-backup`**(1)** to copy and therefore back my pydio cells.

_We advise you to regularly do a backup once it's done it will take lesser time to do the next copy to the same folder will just add the new data onto it_.

Now that the cells installation is backed up if you ever face an issue you can just copy back this folder into `~/.config/pydio/cells` then do a `./cells start` with a new or old binary and you are good to go.

All the informations about the host, the database, plugins configurations etc.. are stored inside this file `~/.config/pydio/cells/pydio.json`

You can also backup all of your database, one way could be to export the database into a sql file another would be to copy the database into another one.

__(1)__ For macos users the path is `~/Library/Application\ Support/Pydio/cells` the `~` being your home path.

### Recovery

As it is explained previously if you have saved all of your cells installation (the `~/.config/pydio/cells` folder) somewhere you can rest assured that it will always be usable, you only need a cells binary and the folder to be put in its default location which is for linux distributions `~/.config/pydio/cells` and for MacOS `~/Library/Application\ Support/Pydio/cells`, and then you can **start** cells `./cells start` if you copied all the folder it's initial location `~/.config/pydio/cells` (the path needs to be the same), cells will use the `pydio.json` to recover the configuration and will run directly you do not need to install it.

