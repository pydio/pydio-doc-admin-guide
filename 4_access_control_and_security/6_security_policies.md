
Security policies allow admin to restrict access to resources and can be created based on lots of criteria:

* the request source IP
* the access time
* the resource type
* the endpoint type
* etc...

When talking of _resources_ in the context of _Security Policies_, it can be:

* a workspace, a file or a folder
* a group or a user 
* a REST API endpoint
* ... 

To gain a deeper understanding of the model, the reader will find these links valuable:

- [IAM Policies - AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html): 
Learn all about AWS Identity and Access Management (IAM) policies and how they work.
- The Github [Ladon](https://github.com/ory/ladon) page: Ladon is the SDK for access control policies that is used uner the hood.

#### How does Security policies work?

A Security policy template is a set of rules that define conditions on subjects to either allow or deny resources access when the condition matches.

Once defined, a single security policy template can be applied on a resource to become active for a given group or user.

Typically when defining access permission via the `Access Controls >> Workspaces Accesses` tab of the `User` or `Group` editor and picking up one template for a given workspace instead of manually setting permissions.

In the  `Security Policies` page of the admin settings, you can configure three types of policies:

##### Context-Based ACLs

These policies are used to dynamically provide read/write access to workspaces or nodes based on the request context and/or the node metadata. They are defined here and used in the Access Control Panel of the users and roles.

##### REST Resources

These policies are protecting the REST APIs on a per-uri / per-method basis. They grant basic access to some specific APIs for public discovery, and a restriction access to many APIs to make sure they are accessed only by frontend application. You should generallynot touch these unless you know exactly what you do.

##### OpenId Connect Resources

OpenId Connect Service is used for authentication of the user, before any access to the APIs. As such, you can totally disable the login operation for a set of users based on the requests context, e.g. disable loging from a given set of IP or at a given time.


#### [ED] Defining a security policy

_Note that creating security policies can be tricky. Make sure you understand well what you do. A misconfiguration can lead to situations in which resources cannot be accessed at all._

*Pydio Cells* comes out-of-box with a carefully chosen set of pre-defined policies for each one of the 3 policy types (see above).

In order to fully understand the concept, we strongly encourage you to check these pre-defined policies and their underlying rules.

To create a new policy template click on the "**+NEW POLICY**" at the top-right. Here is the form description:

* **Name**: This is the name that will be displayed in the various lists, typically when a user picks up a policy to be applied on a workspace.

* **Description**: Text that explains what the policy is about

* **Policy type**: One of the 3 types of policy supported by Pydio Cells.

You then have to create rules for this policy template by clicking on the `ADD NEW RULE` button.

Here you must define:

* **Label**: A self explanatory display name.

* **Effect**: Wether this rule will `allow` or `deny` the action when the condition on context is true.

* **Actions**: Wether this rule is applied on `write`, `read` or both actions  

* **Condition**: One or more JSON objects that enables scripting the effective condition, (see following section to go through so explained examples).

An then save.

To completely remove a Policy Template, click on the `DELETE POLICY` button.


##### Security Policy Examples Explained

**TODO**



