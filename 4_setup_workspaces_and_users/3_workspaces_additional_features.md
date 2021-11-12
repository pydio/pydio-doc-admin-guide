<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP). Time to move to <a href="https://pydio.com/en/docs/administration-guides">Pydio Cells</a>!</span>
</div>

Once the workspace is saved, you can reopen it and see that you have a new tab in the workspace edition dialog, "Additional Features". In this tab, you can add "Metadata" sources support for the workspace. "Metadata" are data that will be attached dynamically to the "core" data of the workspace.

For example, it can be users comments attached to any file/folder of a filesystem, or versioning information, etc. This is "decoupled" from the main access driver of the workspace, because the metadata can use various implementation for the same result. For example, user comments on a file/folder can be saved directly in the filesystem (in hidden files, or in the filesystem metadata if supported), or in an external database, or even why not in the client browser cookies... ?? Below is a schema demonstrating how metadata "enrich" the data.

[:image-popup:4_setup_workspaces_and_users/workspaces_meta_sources.png]

The chapters below will describe the most commons "meta" plugins used to add features to a workspace.

[:summary]
