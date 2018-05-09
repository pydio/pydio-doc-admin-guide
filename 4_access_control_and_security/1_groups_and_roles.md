In the second chapter of this guide, you learnt how to create users, groups and roles.
Now that everything is correctly setup and configured, we will go a step further to show you the adanced capabilities of *Pydio Cells*

### The data model

#### Users

A user represent a single person or entity. The user _object_ provides first and formost the authentication within the system and can also be enriched with metadata, like various information (name, photo...), activities and audit logs.

#### Groups

Groups are a convenient way to organise users hierarchicaly.
A group is defined by its path, for instance `/management/directors` and can be nested under another group. The id of a group is the last part of its path.

When installing the application, a single default group is present: the root group that as a specific `/`path and that is the ancestor of all users and groups.

A user can only be part of one group and is characterised by her login and the path of her group: typically:  `/management/director/jane`

#### Roles 

The role abstraction is the object that 
- authentication
- metadata
- audit    basic  
