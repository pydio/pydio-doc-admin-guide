
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>

Pydio Cells provides an additional layer for visibility management: you can reduce the visibility scope at the _group_ level, meaning that you can choose for instance if users of a group (A) can interact (e.g. see, assign, share with, etc.) with the users of group (B).

### What is group tenancy

In the context of Pydio Cells, group tenancy means that users of a group (A) cannot see the users of another group (B) as long as you respect following rules:

- All groups must be located at the root level (see screenshot below). Group tenancy with nested groups is not implemented in Pydio Cells.

[:image:3_connecting_your_users/group_tenancy/group_tenancy_root_group_level.png]

- To enable this option go to the admin console, **Cells Console > Advanced Settings > Users Visibility**.  

_Note: Users Visibility is an advanced feature. So if you cannot find it where it should be, make sure that you have enabled advanced parameters to be shown in the menu list. This is done by toggling on the `Show advanced parameters button` in the top right corner of the Admin console._

You have 2 settings:

[:image:3_connecting_your_users/group_tenancy/group_tenancy_menu.png]

- **Users visible only to their group:** isolate the various groups.
- **Hide groups from sharing dialogs:** add an additional layer by also removing any mention of groups from the sharing fields.

### For the record

- by default, **every user can see every other user that is defined in your instance**, even if one or the two of them have been nested in a group.
- keep in mind that all users always see the users that have been defined at the root (`/`) level, even if the users that have been defined in 2 distinct non-root groups cannot see each other.
- after switching this option from **Off** to **On** (or the other way round), the changes are applied in background by an asynchronous job: you must wait for the job to finish for the new rules to be effective as it recomputes all user policies. See below a screenshot of the job that computes the policies being processed.

[:image:3_connecting_your_users/group_tenancy/group_tenancy_job.png]

### Examples

If you look at the first screenshot above, any user inside the group **emerald** will still see **admin** or any user at this level, but they will not see users of group **rhinestone** or any other.

Another use case could be a school: let's say you have multiple classes each with its own respective group. You do not want the students of **junior_class_1** to be able to share documents with **junior_class_2** (because one of the classes has a test 2 days before the other, for instance :) ).
