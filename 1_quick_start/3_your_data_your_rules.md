Now that your Cells is up-and-running, you are in control of your data! This section will give you a short overview of users management and access controls, along with the basic sharing capabilities of Pydio Cells.

## Users, Workspaces, Acls

Connect to Cells as admin and go to `Cells Console > People` menu. 

[:image-popup:1_quick_start/yourdata/01-people.png]

Here you can create users and organize them into groups. By default, your data is organized in two Workspaces: "Personal" and "Common Files". The first one is a dedicated "home folder" that you can open to any users: each user will only see their own data inside the Personal workspace. The latter is a shared folder where every one can work on the same files.

Workspaces can be seen as top folders used as basic unit for managing "who can access what". For example, if you edit your own admin user, you can see that you have access to the two default workspaces in read and write mode. 

[:image-popup:1_quick_start/yourdata/02-admin.png]

If you remove the access to Common Files and go back to the home page, you will directly see that you now only have "Personal" accessible in the left-hand column. 

You can manage these access at a fine-grained level (user per user), or a higher level (per group or per role). Actual accesses are computed for each user depending on their roles, groups and personal ACLs.

## Workspaces and Data Source

Now that you understand how to manage access to the data, you may wish to create your own workspace, e.g. to mount an existing folder on your machine, to browse inside Cells. If you go to `Cells Console > Workspaces` and edit the Common Files workspace, you can see that the Data Access section points to an internal path `pydiods1/`. 

[:image-popup:1_quick_start/yourdata/03-commonfiles.png]

This is referring to a "Data Source" named _pydiods1_. Now go to `Cells Console > Storage`, where you will find this datasource and its real path on the file system.

If you wish to mount an existing folder, e.g. `/var/data/mydata`, create a new data source and pick the desired location (for the moment use the `Peer Address` corresponding to your server IP). 

[:image-popup:1_quick_start/yourdata/04-datasource.png]

Once you save the data source, some specific services will start in background, including an indexation of your data. Once it is ready, go back to the `Cells Console > Workspaces` menu and create a "My Data" workspace pointing to this new datasource root. Set default permissions for this workspace (Read and Write for example), which is equivalent to assigning the Read/Write ACL to all users and save. 

[:image-popup:1_quick_start/yourdata/05-newws.png]

Going back to home page, you should now be able to see the workspace and access the data in there!

[:image-popup:1_quick_start/yourdata/06-mydata.png]

[:image:1_quick_start/yourdata/06-mydata.png]

As you may now understand, you could also edit the "Common Files" workspace to point to this new datasource instead of the default one, or even add a second path to this workspace to aggregate data from various locations into one workspace. While Data Sources can be seen as mount points to actual folders on your file system, creating an internal tree of all data, Workspaces are virtual views that can show data from anywhere in that tree and that are used to grant permissions.

A final note, you may have noticed that a data source can be created on the local file system, but also on other remote object storage. See [link to 4_connecting_your_storage/2_datasources]() to learn more about that.

