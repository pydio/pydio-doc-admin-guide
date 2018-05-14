In the second chapter of this guide, you learned how to create users, groups and roles.
Now that everything is correctly setup and configured, we will go a step further to show you the advanced capabilities of *Pydio Cells*.

### The data model

#### Users

A user represent a single person or entity. The user _object_ provides first and foremost the authentication within the system. It can also be enriched with metadata, like various information (name, photo...), activities and audit logs.

#### Groups

Groups are a convenient way to organise users hierarchicaly.
A group is defined by its path, for instance `/management/directors` and can be nested under another group. The id of a group is the last part of its path.

When installing the application, a single default group is present: the root group that as a specific `/` path and that is the ancestor of all users and groups.

A user can only be part of one group and is characterised by her login and the path of her group: typically:  `/management/director/jane`

#### Roles

The role abstraction is the object that is used by the system when you define authorisations and policies. Each user and group is always at least attached with a _canonical_ role that represents it.
Thus, for instance, when you assign specific permissions to a given user, these permissions are assigned under the hood to the canonical role that have the same name.

Roles also adds an indirection level that permits the definition of dynamic _groups_ that will evolve depending on external conditions.

This is a specific Enterprise Distribution feature: starting with Pydio 8, you can import users massively using a Comma Separated file.

### The hierarchy

With Pydio you have a hierarchical system that schematically would look like the pyramidal system you have the top and the bottom, and lets say for instance that you start from the top which would be the user and that you decided that this user will not be able to download, and now in the stage beneath users is groups, and that groups has the download parameter enabled therefore if we follow the rule that the user is on top of the group he will indeed not be able to download, this principle apply to roles as well following this simple scheme as chown bellow :

[image pyramidal system]()

### Importing existing users

This section will describe ways to connect Pydio Cells to your existing users directories.
