<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v1. Looking for <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells v2 docs?</a></span>
</div>

Metadata is a feature that enables the end-user to attach any kind of information to a file or a folder.

Metadata namespaces are predefined by the Admin and their value can be freely added by the end-user. Typically, a good example of predefined metadata is the ability given by the admin to the end user to _rate_ a file or a folder. Another widely used example is the _comments_ feature.

Metadata can be used to enrich files informations. They are visible in the right panel that appear when a file is selected. 

[:image-popup:3_storage_data_and_metadata/file_list_right_panel.png]

You can define almost any type of files metadata and specify for example the order they appear and whether they can be used as search criteria.

In the admin left panel menu click the option metadata in the "Data management" section. And on the On right-top click on the NAMESPACE button to add define one.

### Basic Namespaces Parameters

| Fields    | Descrption                                                                                     |
| --------- | ---------------------------------------------------------------------------------------------- |
| **Name**  | The name of the metada with no space.                                                          |
| **Label** | The name of the metadata as it appears in the interface.                                       |
| **Order** | The position number of the appearance                                                          |
| **Type**  | **Text**, **Long text**, **Starts rate**, **Extensible tags**, **Selection**, **Color Lables** |

Metadata type are various here are some example of use case.

| Meta Type           | Examples                                                             |
| ------------------- | -------------------------------------------------------------------- |
| **Text**            | a small text that is attached to files and appear in file list item. |
| **Long text**       | same as **text** but is bigger                                       |
| **Stars rate**      | rate in a range of 1-5                                               |
| **Extensible tags** | text                                                                 |
| **Selection**       | List of key value pair                                               |
| **Color Labels**    | text                                                                 |

### Visibility Restrictions

In addition of enriching file info, metadata are use in may others part of Cells like the search engine, the security policies, the tasks scheduling and many other features. When defining a metadata the admin can choose  whether or not it is visible or editable by other everyone. The options help adjusting metada restrictions:

| Fields                            | Descrption                                           |
| --------------------------------- | ---------------------------------------------------- |
| **Index in search engine**        | When enabled adds set the metadata as search option. |
| **Restrict visibility to admins** | When enabled only admins users are able to see it    |
| **Restrict edition to admins**    | When enabled only admins users are able to edit it   |

Typically, an admin could define a "Confidential" metadata and let only people with "Admin" status have the right to edit this data. Then the value of this metadata could be used in a security policy to decide whether a document is accessible to the outside network, or outside of office hours, etc.