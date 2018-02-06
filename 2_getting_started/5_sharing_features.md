With Pydio, it is possible to share your folders and files publicly, privately and from version _6.4_, also remotely.

Administration
--------------

[:image-popup:5_customize_users_interface/sharing_features_admin_screens_updated.png]

### Public sharing

#####  Installation

* To enable the ability for users to generate Public Links for their files, simply go to the ‘Sharing features’ screen in the Administration screens, and click on ‘Enable public links for files.

* If you want it to work for folders as well - make sure that one of ‘Minisites and Workspaces’ or ‘Minisites’ is chosen.

#####  Advanced configuration
To do an advanced configuration you have to go to **All Plugins > Available Plugins > Action Plugins > Sharing Features** and edit it by clicking on the pen that's located on the right of this line.

The settings are pretty straighforward so it should be easy to obtain what you wish. You can for example change the number of downloads allowed from a link, give it an expiration time and such.
### Private sharing
	
##### Installation
* This functionality is **enabled** by default.
* If you want to enable / disable the functionality for folders - make sure that one of ‘Minisites and Workspaces’ or ‘Workspaces’ is / is not chosen.

### Remote sharing (Pydio Cloud)

Pydio Cloud is a feature allowing your users to share files or folder directly with other instances of Pydio, and actually other instances of file sharing platforms supporting the Open Cloud API. Major open source EFSS providers are compatible with this API. Use the Pydio Cloud panel to enable this feature. 

[:image-popup:5_customize_users_interface/pydio_cloud_setup_trusted.png]

Pydio provides an advanced feature allowing you to preset a list of "Trusted Servers" that will expose their users directories to your Pydio instance. Currently this is only working between Pydio instances (not part of the OCS Api). To add a trusted server, ask the server admin to create a technical account for you, and use the credentials provided in the configuration.

##### Installation
* To enable the ability for users to share their files privately with one or more users on a separate system, simply go to the ‘Pydio cloud’ screen in the Administration screens, and click on ‘Enable Federated Sharing’.

End-User
--------

The _Share Dialog Box_ is accessible by clicking on the _Share_ button in the Information bar on the right hand side, or from the right click menu, after having selected a file or folder

### Public sharing

[:image-popup:5_customize_users_interface/share_dialog_public_sharing_update.png]

This functionality is useful if you want to share files or folders from your Pydio to multiple users that don’t have an account in your Pydio network. When you activate it, a public url is generated and you can then copy and paste it to share it with anybody with an Internet access. They will be given access to a mini-version of the Pydio interface where they can view the file or folder you wanted them to see.

But if you don’t feel too good about people having free access to your stuff, you can always set permissions so that they cannot _Download_ (nor _Upload_ for folders). You can also set a password that people will need to enter, limit the number of days this link will be accessible for, or set a maximum number of downloads allowed for this particular link.

Still not feeling reassured about that, why not try privately sharing with known Users.

### Users

[:image-popup:5_customize_users_interface/share_dialog_users_update.png]

This functionality is great if you want to share files or folders to users that have an account within your Pydio network (Local users)

Simply go to the _Users_ section of the _Share Dialog Box_, and :

* For **local users**, start entering the username of the person you want to share your stuff with.

Once the file has been shared, or the invitation has been accepted, you will be able modify the permissions assigned to the user (read/write).

### Advanced

[:image-popup:5_customize_users_interface/share_dialog_advanced.png]

For people who know their way round the sharing world, a few advanced perks have been added such as :


* **Description** - You can write a descritpion if you want to.

* **Notification** - If enabled, this will trigger [Alerts](https://pydio.com/en/docs/v6-enterprise/watches-and-notifications) whenever the content has been accessed or modified.

* **Share Visibility** - Enable users wioth whom you share the workspace to view and edit the sharing options you have set up (by default it's private).

### Items shared with me

[:image-popup:5_customize_users_interface/shared_with_me.png]

If you can share files and folders with others, people can also share files or folders with you (tadaaam !) If you’re wondering where to find those items, just look at the left bar (directly visible on  the home page, or by hovering over the button on the left hand side) under the section _Shared with me_.

### Shared Files

If files are shared with you, you will see a Workspace named _SHARED FILES_, where you will find **all files** shared by **all users**, local or remote.

The number of files that have not been seen yet will be shown as a tag.

Approval is required for and only for files that have shared with you remotely. You will see a list of _pending invitations_ at the top and you can either _Approve_ or _Reject_ each from the top bar or the information bar on the right hand side.

If you start having a lot of content shared with you, you can filter it using the search box or the different filter tags on the left hand side. 

### Shared Folders

If you have folders shared with you, you will see multiple Workspaces listed under _SHARED FILES_. Folders that have never been accessed will be tagged _NEW_.

_Only ‘remotely shared items’ are concerned by the following functionality :_

You need to approve folders that have been shared with you by a remote user. Upon access, a warning will be issued asking you to decide whether you want to continue accessing it, or never hear from it again.

