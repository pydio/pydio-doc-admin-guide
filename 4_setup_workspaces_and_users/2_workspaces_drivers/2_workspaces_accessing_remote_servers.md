Pydio comes bundled with plugins to accessing remote file-system based servers via various protocols:

Disclaimer : If you installed Pydio by the linux repository ( apt / yum ), you must install the "pydio-plugin-access.ftp", "pydio-plugin-access.sftp", "pydio-plugin-auth.ftp" packages.

+ **FTP:** use the FTP protocol to access the remote server. You can setup the standard FTP parameters (Port, Active / Passive, etc). Authentication credentials to be identified on the remote server can be passed in various manners (see below).
+ **Sftp:** implement FTP over SSH. This is more stable and more performant than FTP, thus recommended if your server can handle it. However, this requires **libssl2** to be installed (**php_ssh2** extension on Windows).
+ **Samba:** Browse files on a server available via the Samba protocol. This requires smbclient to be installed on the server (tested on \*nix & windows). For linux based servers, access.fs + meta.mount solution should be preferred (using CIFS protocol instead of SMB).
+ **WebDAV:** Browse an http WebDAV server. Warning, do not confuse this feature with the ability of Pydio to serve any workspaces files as through WebDAV to other clients! This driver requires the Pear::HTTP_WebDAV_Client library.
+ **Dropbox:** using this driver, you can create a connexion to an existing Dropbox account. Requires an OAuth implementation (see the plugin identity card). Using "Templates", you could let your users create their own repository, thus entering their own Dropbox keys.

### Passing drivers credentials
When accessing remote servers, most drivers will require an identification using credentials (user / password pair). When configuring a workspace driver, the necessary credentials can be passed in various manners (again):

+ **Setting the credentials at driver level:** all Pydio users accessing the workspace will trigger a remote connexion using a unique shared credential.
+ **Setting the credentials at user level:** once a workspace is created using a "credential-consumer" driver, new fields will appear inside the users editor: that way, it's possible to setup a specific credential for each user.
+ **Using the user's Pydio credentials:** passed through the session, it is possible to directly pass the Pydio credentials to the underlying workspace. For this, you have to set both the "Use Session Credentials" flag in the workspace configuration, and in the Authentication configuration, the "Set Credentials in Session" flag.
