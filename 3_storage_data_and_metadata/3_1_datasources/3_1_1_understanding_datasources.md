
Datasources abstract the effective connection to a given type of data storage at a given address (that might give access to data via a proxy, typically to enable load balancing) using a specific driver.

When adding a new data repository to your Pydio Cells instance, you typically first define a datasource and then define workspace that use it. 

In the following chapters, we will go through the major drivers used to define a datasource. But first, let's go through some common parameters you will have to set up, whatever the driver you select.

### Parameters common to all drivers
**Default Rights** : this settings is very handy to apply automatically an access right to all users for this workspace. If you set it e.g. to "Read", all new users will by default have a Read access to this workspace. This is particularly handy when you are binding Pydio to an external users directory : when a user logs for the first time, his Pydio counterpart is created at this moment, and using default rights is a good way to assign some workspace by defaults for all users.

**Description** : this is now appearing as a legend in the Workspace selector.

**Alias** : this is automatically generated from the workspace label (like the "slugs" in Wordpress for example), and can be used as a workspace identifier for all the low-level operations : building the WebDAV server URLs, triggering an action via the Command Line, etc.

