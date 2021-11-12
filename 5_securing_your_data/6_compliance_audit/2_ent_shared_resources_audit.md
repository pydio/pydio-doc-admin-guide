
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v2. Looking for <a href="https://pydio.com/en/docs/cells/v3/quick-start">Pydio Cells v3 docs?</a></span>
</div>




Pydio Cells provides you with a way to keep track of every shared resources, therefore giving you more visibility and control about who accesses what at any time.

## Audits

With audits you will have all the sharing activity on your cells.
For instance, you deleted a user and you want to retrieve all the shared resources that he had, it's easy you can search for his resources, edit them and change the owner.

Now let's see how we can use this feature,
first generate the audits using this button:

* [:image:5_securing_your_data/auditing_accesses/generate_audit.png]

> a progress bar will appear allowing you to keep track of the progression.

Once the logs are generated, filter them and retrieve the information that you are looking for, you can filter them by:

[:image:5_securing_your_data/auditing_accesses/audit_report.png]

> You can refer to the numbers on the screenshot for each section below.

1. Type of data: Files or Folders only

2. Path Prefix: is the path to the resource; the path prefix can contain a wildcard at the end, the path must be of this form:
   - for a simple case inside a public workspace `<datasource>/<resource>`
   - for a the personal files cases `<datasource>/<username>/<resource>`
   - you can use a wildcard at the end as said for instance `<datasource>/*` or `<datasource>/<folder>/*` and so on.

3. Type of resource: Workspace, Cells or Public links.

4. Owner: by whom the resource is owned.

5. Visible By: users that are able to see the resource.

6. Date: when the resource was updated.


You can also get more details and edit the resources, here the different details on each resource.

### Workspaces

You can retrieve the workspace activity by clicking this button:

[:image:5_securing_your_data/auditing_accesses/workspace_activity_button.png]

it will display the following menu:

[:image:5_securing_your_data/auditing_accesses/workspace_audit.png]

* **node**: the node name (datasource in this case).
* **read access**: the number of users who have read access on the workspace.
* **write access**: the number of users who have write access on the workspace.
* **last update**: the last time the node had an action performed on it.
* **activity**: the history of each and single action that happened on the node.

### Cells

You can retrieve the cells activity the same way you did for the workspaces (by clicking the graph button that will appear when your mouse is on the cells)

[:image:5_securing_your_data/auditing_accesses/cells_audit_options.png]

[:image:5_securing_your_data/auditing_accesses/cells_audit.png]

* **node**: the node name (the path to the cell in this case)
* **owner**: the user that created the cell
* **last updated**: the last time an action happened.
* **activity**: the history of the actions.

You can also edit the cell change it's content and parameters (the same interface as when you create a cell).

### Public link

You can retrieve all the activity that happened around a public link the same way.
(By clicking the graph button on the right of the green icon and name of the owner)
it will display a recap of who accessed the public link and such.

[:image:5_securing_your_data/auditing_accesses/public_link_audit.png]

* **node**: path to the shared resource in this case.
* **owner**: who created the shared link.
* **last updated**: the last time an activity happened.
* **activity**: the activity history (in this case you can see the accesses to the link the cf498178.... is the id of the temporary user created when someone access the resource)
