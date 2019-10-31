# TODO

### Resources policies

Pydio Cells provides an additional layer of collaboration for the end-users: resources like teams, users, but also links and cells, are protected with flexible security rules defining ownership, read and write accesses.

These policies are using the same approach as security policies, with a "Deny By Default" behaviour and a possible set of rules that can be applied to any resources, with users, roles or groups as "Subjects".

Concretely, this means that when a user creates a shared user in her Address Book, she can defines how this new object will be "seen" by other people, and eventually let other people edit this object as well.

### Rules Examples

Users resources are protected by these rules. For example, when the Administrator creates a user, the defaults rules are applied : 

| Resource  | Subject          | Action | Effect | Comment                                         |
| --------- | ---------------- | ------ | ------ | ----------------------------------------------- |
| User.Uuid | profile:admin    | write  | allow  | user is always editable by admins               |
| User.Uuid | profile:standard | read   | allow  | user is visible in address books of other users |
| User.Uuid | user:User.Uuid   | write  | allow  | user must be able to edit himself               |

Where as when a user A creates a shared user B, by default B is not visible to others, thus the rules would be 

| Resource | Subject        | Action | Effect | Comment                                       |
| -------- | -------------- | ------ | ------ | --------------------------------------------- |
| User.B   | profile:admin  | write  | allow  | user is always editable by admins             |
| User.B   | profile:admin  | read   | allow  | user is always visible to admins              |
| User.B   | user:User.Uuid | read   | allow  | user must be able to read his own properties  |
| User.B   | user:User.Uuid | write  | allow  | user must be able to edit himself             |
| User.B   | user:User.A    | read   | allow  | user B is only visible in user A address book |

If User A wishes to share this user with all users of a team XX that she previously created, she could add the following rule to the list

| Resource | Subject          | Action | Effect | Comment                                         |
| -------- | ---------------- | ------ | ------ | ----------------------------------------------- |
| User.B   | role:TeamXRoleId | read   | allow  | let users with role TeamXRoleId see this user B |

Again, this behaviour can be applied in a similar manner to Teams, Cells and Links. We may expand this to Roles and Groups in a near future.

### User Interface

Of course, these rules are not "written" manually by end-users, but we provide an interface for that. Users can find the "Visibility" panel at various places in the interface, for handling exactly that.

#### Users and Teams

Visibility can be set up by end-users via their Address Book.

For users :

[:image-popup:3_identity_management/edit_visibility_user.png]  

[:image-popup:3_identity_management/edit_visibility_popup_user.png]

For teams :

[:image-popup:3_identity_management/edit_visibility_team.png]  

[:image-popup:3_identity_management/edit_visibility_popup_team.png]

#### Links

Managing links visibility will allow the users to let the link they create appear on the files, when inside a workspace shared with other users.

[:image-popup:3_identity_management/shared_link_visibility.png]

#### Cells

As Cells are ways to share data with other users, you can notice that when you select users for sharing, then by default the Visibility rules will grant "Read" access to this Cell to these users. Warning, this "Read" access is just about being able to **read the "metadata" of this Cell**, and is not to be mixed up with the Read/Write **permissions** that define the actual content of the cell.

[:image-popup:3_identity_management/shared_cells_visibility.png]