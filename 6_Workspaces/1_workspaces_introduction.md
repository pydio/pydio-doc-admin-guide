# Introduction #

At this step, you will probably have deeply configured your Pydio installation for all the internal data stores (users, rights, preferences, etc). Now it's time fine-tune Pydio for your actual business data : your files!

One of the core concept of Pydio is the **"Workspace"**. Basically, a workspace can be seen as a virtual drive mounted to access a set of data. In most case, a workspace will access a set of folders and files, locally or remotely via various types of protocol, but it can also be an access to other type of data, like a database content (MySQL repositories), or even the Pydio configurations themselves (Settings workspace). The way the files are accessed is defined when creating the workspace, by choosing a **"Driver"** to map the data.

![workspaces_intro]

The rights to access a given set of data for one or more users is handled at a workspace level. Thus, once you have created a workspace, you will be able to grant read and/or write access to this workspace to your users. You can assign these access individually, but you can also create â€œRolesâ€, a set of right access, and then assign one or more roles to your users.

In the following chapters, we will go through the major drivers used to define a workspace. But first, let's go through some common parameters you will have to set up, whatever the driver you select.

### Parameters common to all drivers
**Default Rights** : this settings is very handy to apply automatically an access right to all users for this workspace. If you set it e.g. to "Read", all new users will by default have a Read access to this workspace. This is particularly handy when you are binding Pydio to an external users directory : when a user logs for the first time, his Pydio counterpart is created at this moment, and using default rights is a good way to assign some workspace by defaults for all users.

**Description** : this is now appearing as a legend in the Workspace selector.

**Alias** : this is automatically generated from the workspace label (like the "slugs" in Wordpress for example), and can be used as a workspace identifier for all the low-level operations : building the WebDAV server URLs, triggering an action via the Command Line, etc.

[workspaces_intro]: ../images/workspaces/workspaces_introduction.png
