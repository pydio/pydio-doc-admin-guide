Metadata is a feature that enables the end-user to attach any kind of information to a file or a folder.

Metadata namespaces are predefined by the Admin and their value can be freely added by the end-user. Typically, a good example of predefined metadata is the ability given by the admin to the end user to _rate_ a file or a folder. Another widely used example is the _comments_ feature.

Metadata can be used to enrich files information. They are visible in the right panel that appear when a file is selected. 

[:image:4_connecting_your_storage/file_list_right_panel.png]

You can define almost any type of files metadata and specify for example the order they appear and whether they can be used as search criteria.

In the admin left panel menu click the option metadata in the "Data management" section. And on the On right-top click on the NAMESPACE button to add define one.

### Basic Namespaces Parameters

| Fields    | Description                                                        |
| --------- | ------------------------------------------------------------------ |
| **Label** | The name of the metadata as it appears in the interface.           |
| **Name**  | The name of the metadata with no space (must start with usermeta-) |
| **Type**  | See available types below                                          |
| **Order** | The position number of the appearance                              |

Metadata type are various here are some example of use case.

| Meta Type           | Description                                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Text**            | A small text                                                                                                                                      |
| **Paragraph**       | Multiline text                                                                                                                                    |
| **Number**          | Integer value<br/>Various format options (General, Bytesize, Percent, Progress)                                                                   |
| **True/False**      | Boolean yes/no values                                                                                                                             |
| **Date/Time**       | Date or Time field (date, time, or date & time).<br/>Can be displayed normaly or as relative to current date (e.g. 4 minutes ago, 2 days ago...). |
| **Stars rate**      | Sequence of clickable stars to rate between 1 and 5.                                                                                              |
| **Extensible tags** | List of values that can be extended<br/>Shows an auto-complete field                                                                              |
| **Selection**       | Preset list of key/value pairs<br/>Each key can also be attached a color.                                                                         |
| **Color Labels**    | Preset list of colored tags (legacy, use Selection instead)                                                                                       |
| **JSON**            | Arbitrary JSON. This type is used by specific plugins to attach data to files.                                                                    |

Below is a preview of how these various types appear in the metadata edition form.

[:image:4_connecting_your_storage/metadata-samples.png]

### Display Options: grouping and ordering

When handling a large number of metadata fields, it may be more user-friendly to group namespaces under categories. That way, fields are more easily findable, and displaying fields inside expandable blocs (closed by default) avoids clutering the UX.

Starting with version 4.2.6, you can create Groups by using the "Group Name" field of a metadata namespace. Once created, it will be available as an auto-complete suggestion in other namespaces. 

[:image:4_connecting_your_storage/meta-grouping-dashboard.png]

Groups can be nested, by using a "slash" (`/`) separator in their name (think of them as folders).

[:image:4_connecting_your_storage/meta-grouping-group.png]

As a result, the "Metadata Info" panel shown to users will look as below: 

<table width="100%" align="center">
<tr>
<td>[:image:4_connecting_your_storage/meta-grouping-closed.png]</td>
<td>[:image:4_connecting_your_storage/meta-grouping-open.png]</td>
</tr>
</table>

Additionally, the **Order** field allows you to fix the order of the fields.

### Visibility Restrictions

In addition of enriching file info, metadata are use in may others part of Cells like the search engine, the security policies, the tasks scheduling and many other features. When defining a metadata the admin can choose  whether it is visible or editable by other everyone. The options help adjusting metadata restrictions:

| Fields                            | Description                                                                       |
| --------------------------------- | --------------------------------------------------------------------------------- |
| **Show in files list**            | Display the metadata value inline directly in the list                            |
| **Index in search engine**        | Indexes values to make them searchable                                            |
| **Restrict visibility to admins** | Only admins users are able to see it                                              |
| **Restrict edition to admins**    | Only admins users are able to edit it                                             |
| **[Ent] Custom Policies**         | Manually edit the restriction rules based on profiles, roles or users identifiers |

Typically, an admin could define a "Confidential" metadata and let only people with "Admin" status have the right to edit this data. Then the value of this metadata could be used in a security policy to decide whether a document is accessible to the outside network, or outside of office hours, etc.