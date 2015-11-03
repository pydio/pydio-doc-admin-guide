Since Pydio 6, the new PydioSync client is now based on a number of server features to make the desktop client lighter and more efficient. Please read go through the basic checks described here to make sure it will work ok.

### How the sync client works

The new python-based PydioSync client is leveraging a Pydio6 unique feature to perform efficient synchronisation with a minimal footprint on both the server and the client. This is achieved by comparing set of “changes” between server and client on a regular basis. The client rarely asks the server for a whole listing of a workspace, instead remembers a given state and ask for changes since that last check. In otherway, instead of requiring “snapshots” of the server, the client requires “delta” of file and folders tree since last check.

Server-side, these “changes” are computed and stored on the fly, on each file/folder modification. Thus, when the client ask for changes, if nothing happened, no compute is performed and the query will just return an empty result, loaded directly from the database.

![protocol]

Last but not least, it’s important to understand that the PydioSync client is using our new REST-based protocol. The first checks will be to make sure that the REST API is correctly working.

### Rest Api

The first important thing to check is that the REST API is correctly working. If you went through the previous chapter of this Administrator Guide, you probably already made sure that the RewriteRules are correctly activated and translated by the server. Otherwise, please [go back to this page now](https://pyd.io/docs/v6/getting-started/check-apis/)! Only once it’s sure you can get further.

Testing URL (on pydio default workspace)

    REQUEST: https://yourserver.tld/pydio/api/default-files/ls/
    ---
    ANSWER:
    <tree repo_has_recycle="true" is_file="false" filename="" mimestring_id="8" icon="folder.png" openicon="folder_open.png" ajxp_mime="ajxp_folder" file_group="1" file_owner="1" file_perms="0777" ajxp_modiftime="1426148120" ajxp_relativetime="Modified yesterday at 09:15" ajxp_description="Modified yesterday at 09:15" bytesize="0" filesize="-" meta_fields="tags" meta_labels="Tags" meta_types="string" text="">
    </tree>
    <tree text="autows10" is_file="false" filename="/autows10" mimestring_id="8" icon="folder.png" openicon="folder_open.png" ajxp_mime="ajxp_folder" file_group="1" file_owner="1" file_perms="0777" ajxp_modiftime="1401974077" ajxp_relativetime="Modified on June, 5th 2014" ajxp_description="Modified on June, 5th 2014" bytesize="0" filesize="-" meta_fields="tags" meta_labels="Tags" meta_types="string" mimestring="Directory"/>
    </tree>
    <tree text="autows13" is_file="false" filename="/autows13" mimestring_id="8" icon="folder.png" openicon="folder_open.png" ajxp_mime="ajxp_folder" file_group="1" file_owner="1" file_perms="0777" ajxp_modiftime="1402565915" ajxp_relativetime="Modified on June, 12th 2014" ajxp_description="Modified on June, 12th 2014" bytesize="0" filesize="-" meta_fields="tags" meta_labels="Tags" meta_types="string" mimestring="Directory"/>
    </tree>
    <tree text="autows18" is_file="false" filename="/autows18" mimestring_id="8" icon="folder.png" openicon="folder_open.png" ajxp_mime="ajxp_folder" file_group="1" file_owner="1" file_perms="0777" ajxp_modiftime="1402248791" ajxp_relativetime="Modified on June, 8th 2014" ajxp_description="Modified on June, 8th 2014" bytesize="0" filesize="-" meta_fields="tags" meta_labels="Tags" meta_types="string" mimestring="Directory"/>
    </tree>

### DB and SQL Triggers set up.

Another key feature needed by our indexation is that your Pydio install is using a DB. If you are using a No-DB install (which is not required in production, but might be the case if you come from Pydio 5), you would have to make sure to provide a DB connexion and install the necessary tables for the indexation to work.

#### _DB was setup during install wizard_

Nothing specific to do here. Tables should be already installed, and SQL Triggers as well. However you can verify that by checking that the ajxp_index and ajxp_changes tables are both present in your database.

#### _DB was not setup during install_

You can configure a Core Connexion in the Configurations Management panel of Pydio, and then manually install the necessary tables by grabing the SQL script corresponding to your DB inside the plugin meta.syncable folder.

#### _Checking Triggers_

In some cases, you can be disallowed to install triggers in your DB, or some specific DB config can break the mechanism. To verify it’s working, you can simply try to manually add a row with fake data inside the ajxp_index table (using your prefered sql client). Once done, you should verify that a new row was created inside ajxp_changes. If you delete the index row, again, you see another new row inside ajxp_changes: on each modification of ajxp_index, a new row is added inside ajxp_changes.

![mysql]

### Workspace configuration
Now that the DB is correctly setup, make sure to enable the “Syncable Workspace (meta.syncable)” plugin in your workspace. This is done via the “Workspace Features” tab of the workspace creation window. For the default workspaces (Default Files and My Files), this should be the case by default, and you can verify this inside the file conf/bootstrap_repositories.php. Finally, make sure that the according parameter “Workspace is Syncable” is set to Yes.

![WS_syncable]

This parameter can be refined by a group/user/role edition, so if you are testing with a given user, make sure she does not have the parameter set to No because of inheritance.

### Server-side indexation
Finally, if you started a fresh new workspace using following the instructions above, as soon as you will start adding data inside this workspace, you will see that the ajxp_changes table is feeding with all events that happened. If the workspace was already containing some contents, you have to make sure to re-index it entirely to make sure ajxp_index (and ajxp_changes) are up to date.

You can request the server changes by calling the following URL.

Where

+ **https://yourserver.tld/pydio/** might depend on your server installation path,
+ **workspace-slug** is the “pretty name” of the workspace. You can see it by logging to this workspace and seeing in the URL of your browser the /ws-workspace-slug/ part (remove ws- to get the slug).
You should receive a json listing all changes since creation of workspace.

    #### REQUEST: https://yourserver.tld/pydio/api/workspace-slug/changes/0
    --
    ANSWER:{"changes":[{"seq":3504,"node_id":2364,"type":"create","source":"NULL","target":"\/logo.png","node":{"bytesize":null,"md5":null,"mtime":null,"node_path":null,"repository_identifier":"1-administrator"}},{"seq":98570,"node_id":2364,"type":"delete","source":"\/logo.png","target":"NULL","node":{"bytesize":null,"md5":null,"mtime":null,"node_path":null,"repository_identifier":"1-administrator"}},{"seq":5362,"node_id":3328,"type":"create","source":"NULL","target":"\/Public Minisite\/Test Folder\/6Capt @ ure.png","node":{"bytesize":null,"md5":null,"mtime":null,"node_path":null,"repository_identifier":"1-administrator"}},{"seq":110546,"node_id":3328,"type":"delete","source":"\/Public Minisite\/Test Folder\/6Capt @ ure.png","target":"NULL","node":{"bytesize":null,"md5":null,"mtime":null,"node_path":null,"repository_identifier":"1-administrator"}},{"seq":5363,"node_id":3329,"type":"create","source":"NULL","target":"\/Public Minisite\/Test Folder\/Again.png","node":{"bytesize":null,"md5":null,"mtime":null,"node_path":null,"repository_identifier":"1-administrator"}},{"seq":110547,"node_id":3329,"type":"delete","source":"\/Public Minisite\/Test Folder\/Again.png","target":"NULL","node":{"bytesize":null,"md5":null,"mtime":null,"node_path":null,"repository_identifier":"1-administrator"}},{"seq":5364,"node_id":3330,"type":"create","source":"NULL","target":"\/Public Minisite\/Test Folder\/CamReview.png","node":{"bytesize":null,"md5":null,"mtime":null,"node_path":null,"repository_identifier":"1-administrator"}},{"seq":110548,"node_id":3330,"type":"delete","source":"\/Public Minisite\/Test Folder\/CamReview.png"," ..............

### Putting things together: testing script
To help you in that task, we’ve setup a script that will try to use the API to create a file inside a workspace, and check if it can find back the “change” afterward. For that, uncomment the die() line of the /runTests.php file and call the following url in your browser : https://yourserver.tld/runTests.php?api=true.

**Please note**: If your server is not located at the document root (e.g. https://yourserver.tld/pydio/), there is a small issue in the current version of this script. Please grab the latest version from github and replace the script before testing (https://raw.githubusercontent.com/pydio/pydio-core/develop/core/src/runTests.php).

![rest]

[protocol]: ../images/getting_started/getting_started_protocol.png
[WS_syncable]: ../images/getting_started/getting_started_ws_syncable.png
[rest]: ../images/getting_started/getting_started_rest.png
[mysql]: ../images/getting_started/getting_started_mysql.png
