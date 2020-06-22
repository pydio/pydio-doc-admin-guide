If you went through the [Quick Admin Tour](./quick-admin-tour), you should already be familiar with the Workspaces concept. Basically a pointer to anywhere in the internal tree aggregated by all Data Sources, they define the basic units to grant or restrict permissions, but also customize actions in the interface etc. These feature will be described in the next sections.

This section will go through the advanced features of Workspaces and explain the difference with 'Cells'.

### Workspaces as a way to organize your data

Workspaces provide a very flexible way to organize your data, depending on who will access to it. It is important to spend some time to define them properly. This is generally a good opportunity for looking at the existing datasets and clean/reorganize them. 

#### Typical layouts

In a typical company setup workspaces will often be organized by business units : 

[:image:5_access_controls/workspaces-layouts.png]

Other will define a per-region organization

- America
- Europe - Middle East
- Asia
- Africa
- etc... 

Up to you. 

#### Multiple Paths workspace

To define a workspace, one must point to at least one location in the DataSources tree. It is interesting to note that you can also compose a workspace from multiple locations. You can that way aggregate various data from various datasources into one unique workspace.

#### Template Paths

One specific workspace is the **Personal** workspace: while defined once and applied globally to all users, this workspace will dynamically create a folder for each users. In other words, each user will always only see her own files while using this workspace. Unless disable, this does not prevent users from sharing data from their Personal workspaces with other people, using Cells or Public Links.

Under the hood, instead of pointing to a defined location in the DataSource tree, this workspace is using a Template Path that is resolved dynamically when accessed. The `my-files` template path is defined by a javascript snippet as follows:   
```
Path = DataSources.personal + "/" + User.Name;
```

In _Cells Enterprise_, you can create your own Template Paths, which can be very useful to e.g. map data from various sources to a unique path. Assuming you have three servers A, B, and C on which users data is evenly distributed (e.g. by their login first letters), you can easily write a script to resolve to the correct data source. Assuming Server A contains users data from [a to h], Server B from [i to p], etc... and you mounted them as separate datasources, script could look like : 

```javascript
// Test first letter of user login
if (['a','b','c','d','e','f','g','h'].indexOf(User.Name[0]) !== -1) {
    Path = DataSources.ServerA + "/" + UserName.Name;
} else if (['i','j','k','l','m','n','o','p'].indexOf(User.Name[0]) !== -1){
    Path = DataSources.ServerB + "/" + UserName.Name;
} else {
    Path = DataSources.ServerC + "/" + UserName.Name;
}
```

### Cells : sandboxed workspaces for the users

As described in the [Sharing Features](./sharing-features) section, Cells are a simple way for users to share data with other users, either by opening them partial access to their personal data, or by creating cells from scratch to start collaborating in a fresh folder. 

In fact, Cells are more or less "just workspaces" that can be managed by users, where as only administrators can manage workspaces. They have the same abilities for aggregating data with multiple paths. Users can create cells based on an existing folders and later on share more data into that same Cells. 

Difference is subtle but important. Workspaces are created by administrators, they are fixed and come with a set of predefined authorizations or restrictions : these rules always be inherited by the Cells created inside those workspaces. As such, Cells provide the flexibility and power of workspaces, empowering your users, while still keeping the data secure and under control.

------
_See Also_

- [Creating a simple workspace in the Quick Admin Tour](./quick-admin-tour)
- [Understanding Data Sources](./understanding-datasources)
- [Assigning workspaces accesses via Users, Groups and Roles](./users-groups-roles)