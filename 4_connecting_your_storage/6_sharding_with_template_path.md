Template paths will give provide you with more sharding over your workspace, you can have a custom and automated tree creation.

### What is a template path

The template path feature is used for the creation of the `personal-files` of each user.
It is defined as the following:

```
Path = DataSources.personal + "/" + User.Name;
```

- `DataSources.<datasourceName>` this can be any datasource of your choice.
- `User.Name` this value is dynamic and will match each user that has a personal-files access.

Which translates into `personal/johndoe` (this is the personal file of the user **John Doe**). 
If you look directly in your storage it will look like this

```
pydio@cells-server:/mnt/data$ tree
.
├── personal
│   └── johndoe
│       └── image.jpg
```
(the full path being `/mnt/data/personal/johndoe`)

The template path is also applied for your personal Cells (the ones that you create without a root).
Now that you understand

### Create a template path

To create a template path, you must first enable the advanced parameters (see screenshot below).

[:image:enable_advanced_parameters.png]

To contextualize the following examples, we have a s3 bucket dedicated to the marketing department and we have 4 branches therefore we would like to split the bucket to isolate each branch.

Head to **Data Management >> Template Paths** and hit the **+ Template** button.

[:image:4_connecting_your_storage/template_path/template_path_new.png]

A popup will appear and ask you to name your template once you have named it hit **create**.


[:image:4_connecting_your_storage/template_path/template_path_edit.png]

Now you can edit your template with the integrated text editor which also has a suggestion feature (macOS: **ctrl + space**).

Once you have finished to edit do not forget to save.