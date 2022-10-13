
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>

By defining quotas to limit the amount of storage that can be used on a workspace basis, this tool grants you more control over the size of your storages therefore allowing you to be cost effective.

### Define quotas on a workspace

Defining a quota on a workspace will restrict the amount of data that you can add. This rule applies globally to everyone that has access to this workspace.

To illustrate the feature, assuming that you have set a quota limit of 200MB and have 20 users each uploading a 10MB file, which will add up to 200MB and therefore reach the limit. Now if any of those users attempts to upload a new file he will not be allowed because the quota was reached.

As a special case, applying a quota of X on a "Personal" workspace, each user will be able to upload up to X amount of data.

To access the menu go to, **Data Management >> Workspaces >> Edit a workspace**, the following menu will be displayed:

[:image:4_connecting_your_storage/assigning_quotas/quotas_workspace.png]

Once you have selected the unit (gigabyte, megabyte and so on) and added the number make sure to hit the save button on your workspace, now the rule will be applied to everyone that has an access to the workspace.

Here is a quick look at what a user will get if they attempt to upload data when the quota limit has been reached.

[:image:4_connecting_your_storage/assigning_quotas/quotas_example.png]