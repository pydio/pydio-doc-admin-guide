<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP). Time to move to <a href="https://pydio.com/en/docs/administration-guides">Pydio Cells</a>!</span>
</div>

As explained in the Introduction to Workspaces, workspace is defined by two things : the "driver" used to access the data (local filesystem, remote ftp server, database, etc...), and the configuration of this driver (the local path to the files, the database credentials, etc). You can even develop your own access driver by following the plugin api. The full list of available drivers for accessing the data is in the "Plugins" part of this documentation, above the "access" category : **https://pydio.com/docs/references/plugins/access/**

To create a workspace, go on your **"Menu" > "Workspaces & Users" > "Workspaces"** in click on **+Workspace** (top of page) and fill the dialog window : use a short, unique and understandable label, and choose the driver that will be used for this workspace. If you don't have an idea of what a driver is, simply choose the "File System (Standard)" driver. Then, depending on the chosen driver, some parameters will appear. All parameters followed by a star "\*" are mandatory. Consult also the "access" plugin documentation on the website. Once your configuration is ok, save the workspace. The workspaces list (left of your screen) should be automatically updated, and you can switch to the new workspace to see if everything is alright (by default, admin user has read/write to all repositories, even newly created).

The following chapter will go through the main drivers available.

[:summary]
