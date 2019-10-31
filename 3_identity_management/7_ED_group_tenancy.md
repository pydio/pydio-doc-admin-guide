# TODO

Pydio Cells provides an additional layer for visibility management: you can reduce the visibility scope at the _group_ level, meaning that you can choose for instance if users of a group (A) can interact (e.g. see, assign, share with, etc.) with the users of group (B).

### What is group tenancy

In the context of Pydio Cells, group tenancy means that users of a group (A) cannot see the users of another group (B) as long as you respect following rules: 

- All groups must be located at the root level (see screenshot below). Group tenancy with nested groups is not implemented in Pydio Cells.

[:image-popup:3_identity_management/group_tenancy_root_group_level.png]

- You have enabled this option by going to the admin console in `left menu >> Advanced Settings >> Users Visibility` (Make sure that you have enabled advanced parameters to be shown in the menu list by toggling the `Show advanced parameters button`in the top right corner of the Admin console) and switching on the **Users visible only to their group** toggle - see screenshot below:

[:image-popup:3_identity_management/group_tenancy.png]


### For the record

- by default, every user can see every other user that is defined in your instance, even if one or the two of them have been nested in a group.
- please keep in mind that all users always see the users that have been defined at the root (`/`) level; even if the users that have been defined in 2 distinct non-root groups cannot see each other.
- after switching this option from **Off** to **On** (or the other way round), the changes are applied in background by an asynchronous job: you must wait for the job to finish for the new rules to be effective as it recomputes all user policies. See below a screenshot of the job that computes the policies being processed.

[:image-popup:3_identity_management/group_tenancy_job.png]

### Examples

If you look at the first screenshot above, any user inside the group **emerald** will still see **admin** or any user at this level, but they will not see users of group **rhinestone** or any other.

Another use case could be a school: let's say you have multiple classes each with its own respective group. You do not want the students of **junior_class_1** to be able to share documents with **junior_class_2** (because one of the classes has a test 2 days before the other, for instance :) ).
