Along with the global administration dashboard, you can use the workspaces editor to monitor information specific to a given workspace, as well as perform some maintenance tasks.

## Workspace Activity

The Activity tab provides an overview of the recent events that happened on the current workspace. The graph shows the total number of files that were uploaded, downloaded and shared in this workspace. As on the main dashboard, you can use the date picker to change the period of the graph.

[:image-popup:3_day_to_day_with_pydio/workspaces-activity.png]

It allows shows a detail of the most recent events on this workspace.

## Shares

You can list and see the details of all shares performed from within this workspace. As an administrator, you also have the ability to totally disable an existing share.

[:image-popup:3_day_to_day_with_pydio/workspaces-shares.png]

## Maintenance

This tab (new in Pydio 7) gives the administrator a way to perform a couple of "maintenance" tasks on the selected workspace. If an indexer is configured for the workspace (which is generally the case by default, using Lucene indexer), you can force a re-indexation of the workspace. If the workspace is defined using a dynamic keyword like AJXP_USER, the indexation will be performed on this workspace for each user.

The panel will also offer you the ability to directly add a task to the scheduler, if you want to e.g. trigger the reindexation every day over night. Please note that this requires the scheduler to be properly configured to work.

The panel will show every currently running tasks.

[:image-popup:3_day_to_day_with_pydio/workspaces-maintenance.png]

You also have the ability to clear all activity (displayed to the user in the right-hand panel of a workspace), which can be useful if you tested Pydio on a staging environment that you now want to put live in production and reset any activity. 