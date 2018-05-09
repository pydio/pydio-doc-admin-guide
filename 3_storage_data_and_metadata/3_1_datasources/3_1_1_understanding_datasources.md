
The rights to access a given set of data for one or more users is handled at a workspace level. Thus, once you have created a workspace, you will be able to grant read and/or write access to this workspace to your users. You can assign these access individually, but you can also create Roles, a set of right access, and then assign one or more roles to your users.

In the following chapters, we will go through the major drivers used to define a workspace. But first, let's go through some common parameters you will have to set up, whatever the driver you select.

### Parameters common to all drivers
**Default Rights** : this settings is very handy to apply automatically an access right to all users for this workspace. If you set it e.g. to "Read", all new users will by default have a Read access to this workspace. This is particularly handy when you are binding Pydio to an external users directory : when a user logs for the first time, his Pydio counterpart is created at this moment, and using default rights is a good way to assign some workspace by defaults for all users.

**Description** : this is now appearing as a legend in the Workspace selector.

**Alias** : this is automatically generated from the workspace label (like the "slugs" in Wordpress for example), and can be used as a workspace identifier for all the low-level operations : building the WebDAV server URLs, triggering an action via the Command Line, etc.

