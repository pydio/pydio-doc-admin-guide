Once the workspace is saved, you can reopen it and see that you have a new tab in the workspace edition dialog, "Additional Features". In this tab, you can add "Metadata" sources support for the workspace. "Metadata" are data that will be attached dynamically to the "core" data of the workspace.

For example, it can be users comments attached to any file/folder of a filesystem, or versioning information, etc. This is "decoupled" from the main access driver of the workspace, because the metadata can use various implementation for the same result. For example, user comments on a file/folder can be saved directly in the filesystem (in hidden files, or in the filesystem metadata if supported), or in an external database, or even why not in the client browser cookies... ?? Below is a schema demonstrating how metadata "enrich" the data.

![meta_sources]

The chapters below will describe the most commons "meta" plugins used to add features to a workspace.

[:summary]

[meta_sources]: ../images/workspaces/workspaces_meta_sources.png
