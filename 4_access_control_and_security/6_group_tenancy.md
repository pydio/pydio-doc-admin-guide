Pydio Cells provides an additional layer for visibility management: you can define some scopes at the _group_ level, meaning that you can choose for instance if users of a (A) group can interact (e.g. see, assign, share with, etc.) with the users of (B) group.

### What is group tenancy

In the context of Pydio Cells, group tenancy means that users of a (A) group cannot see the users of the (B) group, as long as you match the list of requirements. 

For the record: 

- by default, every user can see every other user that is defined in your instance, even if one or the two of them have been nested in a group.
- please keep in mind that all users always see the users that have been defined at the root (`/`) level; even if the users that have been defined in 2 distinct non-root groups cannot see each other.

**Example 1 :**

For instance if we look at the first screenshot (below) any user inside the group **emerald** will still see **admin** or any user at this level, but they will not see users of group **rhinestone** or any other.

**Example 2 :**

Another case would be a school, you have multiple classes each with its own respective group and you do not want students of **junior_class_1** to be able to share documents with **junior_class_2** (because they got the test in advance).

### Requirements and configuration

To make use of group tenancy, groups must all be located at the root level as seen in this screenshot.

[:image-popup:4_access_control_and_security/group_tenancy_root_group_level.png]

> Groups must all be located at the root level to be acknowleged by the policy (see screen above).

Once you have all the requirements go inside the menu(left bar menu, _make sure that you have enabled advanced parameters to see the complete list_) **Advanced Settings > Users Visibility** and enable **Users visible only to their group**.

[:image-popup:4_access_control_and_security/group_tenancy.png]

_**Once you have the setting enabled, the changes are applied in background by an asynchronous job: you must wait for the job to finish for the new rules to be effective as it recomputes all user policies**_

Screenshot of the task that computes the policies.

[:image-popup:4_access_control_and_security/group_tenancy_job.png]
