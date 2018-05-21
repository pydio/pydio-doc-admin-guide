Now that you have defined your datasources, you can merge and organise the various views over your data at a business level. That is the main goal of workspaces.

Workspaces are top-level folders helping you organize your data. They may be accessed only by a given user or shared accross many people.

To simplify, a workspace can cannonically expose a subtree of any of the filesystems that is provided by one of your datasources, but can also gather transparently data from heterogeneous sources or mounting points in a single data source: you may see it like a folder containing symbolic links on a linux-like file system.

In this section, we will introduce the most widely used types of workspace namely:

- basic workspaces that refers only to a subtree of a datasource
- personal file workspaces, and more generally speaking template-path-based workspaces

From an admin point-of-view, Workspaces will generally be the basic unit to grant accesses to any part of the tree to users. You will see how this apply in this chapter and how users, groups and roles can make this really flexible.

### Standard workspace

As you have already seen in [chapter 2](/en/docs/cells/v1/getting-started) of this guide, it is straight forward and easy to define a simple workspace: please refer to this [section](/en/docs/cells/v1/creating-workspaces) for a step by step guide.

You then have a direct access to the root of the underlying datasource and can start using it.

**About the Default Rights**: the default rights is a specific policy that is applied to the "Root Group" of the application, which mean to all users. If you create or edit a new workspace and select the Read-Write option, all your user will see this workspace appears in there workspace list (in the left menu) and can start uploading and modifying files. This can then be overriden on a per-user, per-group or per-role basis (see [Chapter 4](/en/docs/cells/v1/access-control-and-security)).

**About the Recycle Bin**: The Recycle Bin is a specific folder created at the first connection to this workspace. When present, the "Delete" action will in fact be replaced by a "Move To Recycle Bin" action. Only inside the recycle bin itself will the Delete action be actually performed.

### Personal Workspaces

The Personal workspace is very similar to the `home` or `My Files` folders that you can find in various operating systems. You create one workspace and assign the right to use it to all users, but the actual data shown to user will be their own and never accessible by other users.

Under the hood, the _"physical"_ effective path toward the default node that is used for storing user personal data in separate folders is dynamically computed based on various contextual aspects. You might change see the rule that determines this path by going to  `Admin Settings >> left-hand menu >> Template Paths >> MY-FILES`. In the **Enterprise Edition** you can edit this value. 

As seen there, the default path is defined by `Path = DataSources.personal + "/" + User.Name;` where:

- **DataSources.personal** is a default built-in file system datasource that points towards the `<cells data dir>/data/personal` data sub directory of your Pydio installation
- **User.name** is the login of the corresponding user

Thus, although it seem that all users access the "same" workspace, this workspace is actually always pointing to a different folder, depending on the user logged.

### Template Paths

More generally speaking, Template Paths are dynamically computed depending on the context. They can be used as roots for workspaces in replacement of a fixed datasource path.

As quoted above, it is used to create the Personal Files workspace. It is also used to determine the location of the users Cells folders: as we have already seen in previous sections, the Cells concept adds an indirection level that brings us the possibility to gather nodes from various sources. But when a user creates a Cell from scratch and without any existing folder, we have to provide a folder to start storing contents. This is were the Cells template path is used, to create different folders for each Cell, sharded amongst user logins.

Template Paths can be used for sharding data accross many datasources, or to point to existing server based on e.g. a user ID.

#### [ED] Create and manage specific Template Paths

In Cells **Enterprise Edition**, in addition to the two template paths presented above (namely for the `My Files` and `Cells` folders), you can define dynamic folders of your own, depending on your business needs.

In the page of the admin settings where you configure Personnal Folders and Cells: `Admin Settings >> left-hand menu >> Template Paths`. Click on the top right `+TEMPLATE PATH` button to create new custom template.

The syntax used to create Template Paths is **JavaScript**. The embedded editor provides a contextual auto-completion feature exposing available variables for ease of use. Basically, the role of the script is to compute the output of the `Path` variable depending on the context.

##### Example 1 : Personal Payslip

Here is a complete step by step example to illustrate the use of this feature.

Let's say for instance that you are configuring Pydio Cells for an enterprise and that you want to give to each employee an access to her own payslips.

First you have to:

- create a (encrypted) datasource that point to a secure storage, here we will name it: `administrativeDocs`
- create a workspace called `Admin Docs` that will use this datasource
- give access to this workspace only to the _"Accounting"_ and _"Management"_ groups  
- create a subfolder named `Payslips` in this workspace

Then you can:

- create a new rule by clicking on the `+ DYNAMIC NODE` button that is in the top right corner of the page.
- name it `Payslip folders` for instance
- edit the default path using: `DataSources.administrativeDocs + "/Payslips/" + User.Name;`
- save 

##### Example 2: Users distributed on X servers

Organisations with huge number of users generally will store their "home" folders on various fileservers. Assuming you have 4 servers where users are distributed as follow : 

- Server 1: Users from A to J
- Server 2: Users from K to P
- Server 3: Users from Q to Z

On each server, folders are stored under /homes/a/azerty, /homes/b/bvcxqf, where azerty and bvcxqf would be a user logins.

You would have defined 3 datasources pointing to (and name respectively) server1, server2 and server3 (exposing the /homes folder for each).
 
You could use the following template path script :
```
var first = User.Name.charAt(0);
var ds = "";
if (first < 'K') {
    ds = DataSources.server1;
} else if (first < 'Q') {
    ds = DataSources.server2;
} else {
    ds = DataSources.server3;
}
Path = ds + "/" + first + "/" + User.Name + "/home";
``` 

The computed Path would then dynamically point to the right folder.
