Template paths will provide you with more sharding over your storages, you can have a custom and dynamic tree creation.

### Template path

One specific workspace is the **Personal** workspace: while defined once and applied globally to all users, this workspace will dynamically create a folder for each users. In other words, each user will always only see her own files while using this workspace. Unless disable, this does not prevent users from sharing data from their Personal workspaces with other people, using Cells or Public Links.

Under the hood, instead of pointing to a defined location in the DataSource tree, this workspace is using a Template Path that is resolved dynamically when accessed. The `my-files` template path is defined by a javascript snippet as follows:   
```
Path = DataSources.personal + "/" + User.Name;
```

- `DataSources.<datasourceName>` this can be any datasource of your choice.
- `User.Name` this value is dynamic and will match each user that has a personal-files access.

Which translates into `personal/johndoe` (this is the personal file of the user **John Doe**). 
If you take a closer look in your storage it will end up like this:

```
pydio@cells-server:/mnt/data$ tree
.
├── personal
│   └── johndoe
│       └── image.jpg
```
(the full path being `/mnt/data/personal/johndoe`)

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

There is also template path for your personal Cells (the ones that you create without a root).

Now that you understand the concepts, lets create our own template path.



### Create a template path

To create a template path, you must first enable the **advanced parameters** (see screenshot below).

[:image:enable_advanced_parameters.png]

To contextualize the following examples, we have a s3 bucket dedicated to the marketing department and we have 4 branches therefore we would like to split the bucket to isolate each branch.

Head to **Data Management >> Template Paths** and hit the **+ Template Path** button.

[:image:4_connecting_your_storage/template_path/template_path_new.png]

A popup will appear asking you to name your template once it is done hit the **create** button.

[:image:4_connecting_your_storage/template_path/template_path_edit.png]

Now you can edit your template with the integrated text editor which also has a suggestion feature (macOS: **ctrl + space**).

Once you have finished to edit do not forget to save.
