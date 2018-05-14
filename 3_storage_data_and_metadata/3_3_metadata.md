
Metadata is a powerful feature that enables the end-user to attached any kind of information to a file, a folder or a whole workspace.
Metadata can be predefined by the Admin or freely added by the end-user. They are also automatically added by the system, for instance when storing access and modification history of a given node.

These metadata will be attached dynamically and tranparently to the "core" data of the underlying object.

Typically, a good example of predefined metadata is the ability given by the admin to the end user to _rate_ a file or a folder.

Another widely used example is the _comments_ feature.

This is "decoupled" from the main access to the data, because the metadata can use various implementation for the same result. For example, user comments on a file/folder can be saved directly in the filesystem (in hidden files, or in the filesystem metadata if supported), or in an external database.

The chapters below will describe the most commons "meta" plugins used to add features to your Pydio Cells instance.

### File Metadata

Metadata can be used to enrich files informations. They are visible in the right panel that appear when a file is selected. 

![meta](/images/3_storage_data_and_metadata/file_list_right_panel.png)


You can define almost any type of files metadata and specify for example the order they appear and whether they can be used as search criteria.

In the admin left panel menu click the option metadata in the "Data management" section. And on the On right-top click on the NAMESPACE button to add define one.


| Fields| Descrption|
|-------|-----------|
|**Name**| The name of the metada with no space.|
|**Label**| The name of the metadata as it appears in the interface.
|**Order**| The position number of the appearance|
|**Type**|  **text**, **long text**, **starts rate**, **Extensible tags**, **Selection**, **Color Lables**|

Metadata type are various here are some example of use case.

| Meta Type| Examples|
|----------|---------|
|**text**| a small text that is attached to files and appear in file list item.|
|**long text**| same as **text** but is bigger|
|**stars rate**| rate in a range of 1-5|
|**Extensible tags**|  text|
|**Selection**|  List of key value pair|
|**Color Labels**|  text|


Options:

In addition of enriching file info, metadata are use in may others part of Cells like the search engine , the security policies, the tasks scheduling and many other features. When defining a metadata the admin can choose  whether or not it is visible or editable by other everyone. The options help adjusting metada restrictions:


| Fields| Descrption|
|-------|-----------|
|**Index in search engine**| When enabled adds set the metadata as search option.|
|**Restrict visibility to admins**| When enabled only admins users are able to see it|
|**Restrict edition to admins**| When enabled only admins users are able to edit it|