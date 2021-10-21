
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v2. Looking for <a href="https://pydio.com/en/docs/cells/v3/quick-start">Pydio Cells v3 docs?</a></span>
</div>




The Storage Usage tool allows you to visualize and understand where most of your storage is being used, and what type of resource is using most of it.


## Storage Usage

To visualize the storage usage head to, **Cells Console >> Audits**, and select the **Storage** tab.

The following menu will appear displaying the storage usage of all the available datasources on your Cells instance.

[:image:5_securing_your_data/auditing_accesses/storage_usage/storage_loaded.png]

## How to use the Storage usage audit

When you first visit the storage menu only the main datasources size are loaded we, do not load everything because it takes resources to compute the sizes of the underlying tree to give you a more detailed view therefore we do it progressively as you browse deeper into the storage.

[:image:5_securing_your_data/auditing_accesses/storage_usage/storage_not_loaded.png]

To load the storage repartition for `cellsdata`, click on the `cellsdata` part, then you will have more detail (see screenshot below)

[:image:5_securing_your_data/auditing_accesses/storage_usage/storage_detail_cellsdata.png]

and then you can still go a level deeper, to analyse the usage for a specific user, here for instance we are going to look at the user `admin` _cell_ storage usage.

[:image:5_securing_your_data/auditing_accesses/storage_usage/storage_detail_cellsdata_admin.png]


We could also analyse the `personal` workspaces of your current users and see who and what is taking so much space as seen on the screenshot below.

[:image:5_securing_your_data/auditing_accesses/storage_usage/storage_detail_personal.png]