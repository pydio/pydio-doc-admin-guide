## Group tenancy

Pydio Cells provides an additional layer for visibility by giving you the ability to restrain the scope for groups, meaning that you can choose if users of group A can interact (share for instance, see and else) with users of group B.

### What is group tenancy inside Pydio Cells

In the context of Pydio Cells group tenancy means that users of Group-A cannot see users of Group-B (as long as you match the list of requirements), by default the behaviour is that any user even if sorted inside a group, can see any other user inside Pydio Cells.

> be advised that even if users inside group will not see each other, they will always see the users inside the root level.

**Example 1 :**

For instance if we look at the first screenshot (below) any user inside the group **emerald** will still see **admin** or any user at this level, but they will not see users of group **rhinestone** or any other.

**Example 2 :**

Another case would be a school, you have multiple classes each with its own respective group and you do not want students of **junior_class_1** to be able to share documents with **junior_class_2** (because they got the test in advance).

### Requirements and configuration

To make use of group tenancy, groups must all be located at the root level as seen in this screenshot.

[:image-popup:5_advanced/root_group_level.png]

* groups must all be located at the root level to be acknowleged by the policy (see screen above).

Once you have all the requirements go inside the menu(left bar menu, _make sure that you have enabled advanced parameters to see the complete list_) **Advanced Settings > Users Visibility** and enable **Users visible only to their group**.

[:image-popup:5_advanced/group_tenancy.png]

_**Once you have the setting enabled, a task will be launched and therefore you must wait for the job to completly finish (the job recomputes the users policies)**_

[:image-popup:5_advanced/group_tenancy_job.png]
