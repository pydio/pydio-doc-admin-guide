In the second chapter of this guide, you learnt how to create users, groups and roles.
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




### Importing existing users 

This section will describe ways to connect Pydio Cells to your existing users directories.

#### [ED] Import users from CSV 

This is a specific Enterprise Distribution feature: you can import users massively using a Comma Separated file.

##### Prepare the CSV  

Using a text editor or a spreadsheet editor, prepare the data that you will import with at least the following columns: 

 - **login** user identifier used to login
 - **password** user password

You can also add other columns containing the following data : 

 - **name** The display name showed to other users
 - **email** an email address
 - **role** a pydio role to be applied at import when creating the user. If it does not already exists in Pydio, it will be created.

You don't have to name these columns, the importer provides a step to map the columns to the expected data. That said, you can use the first line of the file as header, the import process lets you ignore the first line. 

Below is a sample file prepared for import:

[:image-popup:2_getting_started/csv_importer/import_users_csv_sample.png]

Make sure to set up the correct "delimiter" and "separator" when exporting to CSV. Default values are generally respectively "quotes" and "semi-column."

##### Load file in Importer

In your `Admin Dashboard > People` section, use the third button in the header to open the CSV importer

[:image-popup:2_getting_started/csv_importer/import_users_start.png]

Drop the CSV file you have prepared or open the picker to select it from your computer. It will be uploaded and the first lines will be displayed to setup the mapping.
There, you can see each columns, and choose the selector in each column header to tell what data it is containing. If you have extra data, you can select "Ignore" as well.

[:image-popup:2_getting_started/csv_importer/import_file_parsed.png]

If you have a line of headers in your CSV, you can ignore it by checking the box at the bottom of the screen.

##### Start Importing

Once you are ready, start importing and the task will run in background.

[:image-popup:2_getting_started/csv_importer/import_users_finished.png]

You should see the various users creation in the bottom right notification bar.

[:image-popup:2_getting_started/csv_importer/import_users_task.png]

##### Refresh the list

The list is not automatically refreshed. Hit the refresh icon in the top right of the list to see the users and start assign rights to them!
 
