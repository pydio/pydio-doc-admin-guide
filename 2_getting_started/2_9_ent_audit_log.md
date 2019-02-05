In the **Enterprise Edition**, an audit log gathers all non-technical events to help focus on business logic when analysing logs for compliance or when tracing events after a security breach.

Audit Logs are accessible under the section **Identity Management** inside the menu **Audits**.

Logs are grouped using a unique Request-Id as they may be produced by a unique action going through various micro-services.

When using Supervisorctl service, audit logs are also available in the `~/.config/pydio/cells/logs/audit.log` file (rotated every day).

## Exporting logs

For an easier analysis, audit logs can be exported in XLSX or CSV format.  
To do so, you can use the button in the top-right of the audit logs screen. Please note that you must first filter the logs (either by searching or by restricting to a given period) to avoid exporting the whole base of logs at once.

## Audit logs

First you must generate the audit log using this button:

* [:image-popup:2_getting_started/audit_log/generate_audit.png]

> a progress bar will appear so that you can keep track of the progression.

Once the logs are generated you will be able to filter them and get the information that you want, you can filter them:

[:image-popup:2_getting_started/audit_log/audit_log_interface.png]

1. Type of data: Files or Folders only

2. Path Prefix: is the path to the resource; the path prefix can contain a wildcard at the end, the path must be of this form:
   * for a simple case inside a public workspace `<datasource>/<resource>`
   * for a the personal files case or such `<datasource>/<username>/<resource>`
   * you can use a wildcard at the end as said for instance `<datasource>/*` or `<datasource>/<folder>/*` and so on.
> the following screenshots will illustrate the path filter for you.

[:image-popup:2_getting_started/audit_log/filtering_example1.png]

[:image-popup:2_getting_started/audit_log/filtering_example2.png]

[:image-popup:2_getting_started/audit_log/filtering_example3.png]

3. Type of resource: Workspace, Cells or Public links.

4. Owner: by whom the resource is owned.

5. Visible By: users that are able to see the resource.

6. Date: when the resource was updated.


You can also get more details and edit the resources, here the different details on each resource.

### Workspaces

You can retrieve the workspace activity by clicking this button:

[:image-popup:2_getting_started/audit_log/workspace_activity_button.png]

it will display the following menu:

[:image-popup:2_getting_started/audit_log/workspace_activity.png]

* node : the node name (datasource in this case).
* read access: the number of users who have read access on the workspace.
* write access: the number of users who have write access on the workspace.
* last update: the last time the node had an action performed on it.
* activity: the history of each and single action that happened on the node.

### Cells

You can retrieve the cells activity the same way you did for the workspaces (by clicking the graph button that will appear when your mouse is on the cells)

[:image-popup:2_getting_started/audit_log/cells_activity.png]

* node: the node name (the path to the cell in this case)
* owner: the user that created the cell
* last updated: the last time an action happened.
* activity: the history of the actions.

You can also edit the cell and change it's content and parameters (the same interface of when you create a cell).

### Public link

You can retrieve all the activity that happened around a public link the same way.
(By clicking the graph button on the right of the green icon and name of the owner)
it will display a recap of who access the public link and such.

[:image-popup:2_getting_started/audit_log/public_ling_activity.png]

* node: path to the shared resource in this case.
* owner: who created the shared link.
* last updated: the last time an activity happened.
* activity: the activity history (in this case you can see the accesses to the link the cf498178.... is the id of the temporary user created when someone access the resource)
