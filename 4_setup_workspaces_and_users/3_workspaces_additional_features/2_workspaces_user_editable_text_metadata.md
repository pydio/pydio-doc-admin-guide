A common need for a system like Pydio is to let the users enter their own comments or remarks about the files they use. Pydio lets you simply define as much metadata fields as you want, with various field types available, through this plugin **meta.user**.

**This plugin requires a "metastore"** plugin to be active to do the actual storing implementation. If you are not sure of what this mean, just read the previous section. Metastore will handle the actual storing of the metadata, where as the plugin will handle how it is displayed and other users interface.

Metadata is attached to a file, which means it is moved/copied/deleted between folders when such operations are applied on the files. The plugin provides a little editor for adding/modifying these properties as simple text lines, display them in both the file list and in the infoPanel, and the added metadata is also searchable.

To add some metadata fields, simply follow the steps:

+ In the "Additional Features" of a workspace configuration panel, make sure a MetaStore is defined, and add a "Text Metadata" plugin to the stack.
+ You can see a replicable set of three fields:
    - Meta Field: a technical name for the field. Please use only alphanumerical characters. See below for specific keywords.
    - Meta Label: will be displayed to the user
    - Column visibility: can be "visible" or "hidden" (without quotes). This defines whether in List mode, the associated will be visible by default or not.

Some special keywords can be used to name metadata fields, that will behave specially in terms of GUI:

+ **stars_rate:** the field automatically turns into a 5-stars rating system for the files. View and edit the rating directly in the InfoPanel, in the FilesList (in list mode only for the moment), and in the "File Meta Data" form.
+ **css_label:** the field turns into a labeling system that will apply a given css class to either the table row when in list mode, or the thumbnail cell when in thumb mode. The css classes are defined in plugins/meta.serial/css and by default they define 5 levels of priority with associated background colors. Use png transparent images as background colors, that way the user can still notice when a row is selected or not.
+ **area_XXX:** Prepending "area_" to a field name will create a text-area field.

For example, here is below a correspondence between a set of configuration and the result for the end user:

[:image-popup:6_workspaces/workspaces_user_editable_text_metadata.png]
[:image-popup:6_workspaces/workspaces_user_editable_text_metadata2.png

Will give:

[:image-popup:6_workspaces/workspaces_user_editable_text_metadata_result.png]
