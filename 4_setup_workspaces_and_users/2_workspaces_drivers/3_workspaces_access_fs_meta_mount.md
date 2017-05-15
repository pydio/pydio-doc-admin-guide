<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 7. Looking for <a href="https://pydio.com/en/docs/v8/accessfs-metamount">Pydio 8 docs?</a></span>
</div>

### The mount command line

> Warning, this is only implemented for \*nix servers!

Accessing a remote server can be done using the existing protocols like FTP, FTP over SSH, etc. But using these drivers may present some limitation in terms of available additional features. Most interesting features are generally developed by default for a local file system.

A good option can thus be to use existing OS capabilities to actually mount the remote drive as a "local" folder. On Linux, you are used to do that with the "mount" command, that will use kernel or FUSE-based modules to load remote FS as local folders. But as generally the remote servers must be provided with credentials (see previous chapter), the mount operation may require the same type of credentials passing, as well as eventually mounting a given folder just for the time where the user is logged, and unmount it automatically afterward.

This is exactly the mechanism "meta.mount" is implementing : dynamically calling a "mount -t" command at user login (where when a user switches to a workspace) and unmounting the folder when the user logs out. The typical case we recommend, whenever possible, is for example to replace the Access.smb driver by a CIFS dynamic mounting setup. This is the setup described below, and can easily be adapted for other FS types.

### Setup a dynamic CIFS access
In our case, we assume to have the following : an SMB server accessible at 192.168.0.156, via the CIFS protocol, that presents a "/username" share for mounting. We want to access it through an access.fs driver instead of an access.smb. We want the mounted folder to be dynamically mounted and unmounted when user enters / exits the workspace.

Also we assume that we will create Pydio users with the same credentials as the ones they would provide to access the SMB share directly.

+ **Create a new workspace**, selecting the Standard FS driver
+ **Setup the Path** to point to a folder containing the AJXP_USER keyword : e.g; **_/mnt/ajxp/AJXP_USER_**
+ **Set the "Create" option to True**. The various /mnt/ajxp/user1, /mnt/ajxp/user2 will be created automatically.
+ Set up other options of the driver as you like. Save.
+ Add a "Repository Feature" by choosing "FS Mount" in the Meta plugin list.
+ Configure the FS Mount plugin as follow :
    - **FS Type** : cifs
    This is the value that will appear after the "-t" of the mount command
    - **Sudo** : yes or no ?
    Generally, this mount operation should be performed by a super user. A good way is to use SUDO to achieve this. You would allow the web server user (e.g. httpd, www-data) to perform both mount & unmount commands. On Debian this would add the following line in the /etc/sudoers file :
    `www-data ALL = NOPASSWD: /bin/umount,/bin/mount`
    - **Remote path : _//192.168.0.156/AJXP_USER_**
    Enter samba-like path to the share.
    - **Mount options : _user=AJXP_USER,pass=AJXP_PASS,uid=www-data,gid=www-data_**
    Add various options the current mount protocol can handle. Added after the "-o" options. Here we want to dynamically pass the user & pass through the mount command.
    - **User / Password** : leave empty
    - **Session Credentials** : Yes.
    We want to use the Pydio credentials for logging to the SMB share. Don't forget to also check the "Set Credentials in Session" flag in the main Authentication configuration panel.

That's it. Save these option, cross your fingers, and try accessing to this new workspace.

Common problems will generally come from the SUDO configuration, or if you are using Session Credentials, don't forget you must use a user that is bother authenticated on Pydio and on the remote SMB server.
