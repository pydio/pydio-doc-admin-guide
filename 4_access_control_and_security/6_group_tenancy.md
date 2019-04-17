Pydio Cells provides an additional layer for visibility by giving you the ability to restrain the scope for groups, meaning that you can choose if users of any group can interact (share for instance, or everything else) with users of other groups.

### What is group tenancy inside Pydio Cells

In the context of Pydio Cells group tenancy means that users of Group-A cannot see(interact, share, ...) with users of Group-B (as long as you match the list of requirements), by default the behaviour is that any user even if sorted inside a group, can see(interact, share, ...) any other user inside Pydio Cells.

> Be advised that even if users inside groups will not users of other groups, they will still see the users inside the root level (see example 1).

**Example 1 :**

For instance if we look at the first screenshot (below) any user inside the group **emerald** will see **admin** or any user at this level, but they will not see users of group **rhinestone** or any other group (at root level).

**Example 2 :**

Another case would be a school, you have multiple classes with each its own respective group and you do not want students of **Junior_Class_1** to be able to share documents with **Junior_Class_2** (because they got the test in advance).

### Requirements and configuration

To make use of group tenancy, groups must all be located at the root level as seen in this screenshot.

[:image-popup:4_access_control_and_security/group_tenancy_root_group_level.png]

> Groups must all be located at the root level to be acknowleged by the policy (see screen above).

Once you have all the requirements met,
go inside the menu (left bar menu, _make sure that you have enabled advanced parameters to see the complete list_) **Advanced Settings > Users Visibility** and enable **Users visible only to their group**.

_**Please make sure to understand that once the setting is enabled it will apply to every exisiting group as long as they matche the requirement, (you cannot single out only **one chosen** group).**_

[:image-popup:4_access_control_and_security/group_tenancy.png]

_**When the setting is enabled, a task will be start, you must wait for the job to completly finish (the job recomputes the users policies)**_

Screenshot of the task that computes the policies.

[:image-popup:4_access_control_and_security/group_tenancy_job.png]
