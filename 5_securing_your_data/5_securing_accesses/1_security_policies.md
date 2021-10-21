
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v2. Looking for <a href="https://pydio.com/en/docs/cells/v3/quick-start">Pydio Cells v3 docs?</a></span>
</div>




Visit the knowledge base for examples of application for the security policies.	

- [ACL use cases](en/docs/kb/access-control/acl-use-cases)	

Security policies allow admin to restrict access to resources and can be created based on lots of criteria:	

- Request source IP	
- Access time	
- Resource type	
- Endpoint type	
- etc...	

When talking of _resources_ in the context of _Security Policies_, it can be:	

- A file or a folder, and by extension a workspace, via ACLs	
- A REST API endpoint	
- An OpenId Connect operation	

To gain a deeper understanding of the model, the reader will find these links valuable:	

- [IAM Policies - AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html): learn all about AWS Identity and Access Management (IAM) policies and how they work.	
- The Github [Ladon](https://github.com/ory/ladon) page: Ladon is the SDK for access control policies that is used under the hood.	

## How do Security Policies work?	

A Security policy template is a set of rules that define conditions on subjects to either allow or deny resources access when the condition matches.	

Once defined, a single security policy template can be applied on a resource to become active for a given group or user.	

Typically when defining access permission via the `Access Controls >> Workspaces Accesses` tab of the `User` or `Group` editor and picking up one template for a given workspace instead of manually setting permissions.	

In the  `Security Policies` page of the admin settings, you can configure three types of policies:	

### Context-Based ACLs	

These policies are used to dynamically provide read/write access to workspaces or nodes based on the request context and/or the node metadata. They are defined here and used in the Access Control Panel of the users and roles.	

### REST Resources	

These policies are protecting the REST APIs on a per-uri / per-method basis. They grant basic access to some specific APIs for public discovery, and a restriction access to many APIs to make sure they are accessed only by frontend application. You should generally not touch these unless you know exactly what you do.	

### OpenId Connect Resources	

OpenId Connect Service is used for authentication of the user, before any access to the APIs. As such, you can totally disable the login operation for a set of users based on the requests context, e.g. disable logging from a given set of IP or at a given time.	

## [ED] Defining a security policy	

_Note that creating security policies can be tricky. Make sure you understand well what you do. A misconfiguration can lead to situations in which resources cannot be accessed at all._	

Pydio Cells comes out-of-box with a carefully chosen set of pre-defined policies for each one of the 3 policy types (see above).	

In order to fully understand the concept, we strongly encourage you to check these pre-defined policies and their underlying rules.	

To create a new policy template click on the "**+NEW POLICY**" at the top-right.  	
Here is the form description:	

- **Name**: This is the name that will be displayed in the various lists, typically when a user picks up a policy to be applied on a workspace.	
- **Description**: Text that explains what the policy is about	
- **Policy type**: One of the 3 types of policy supported by Pydio Cells.	

You then have to create rules for this policy template by clicking on the `ADD NEW RULE` button.  	
Here you must define:	

- **Label**: A self explanatory display name.	
- **Effect**: Wether this rule will `allow` or `deny` the action when the condition on context is true.	
- **Actions**: Wether this rule is applied on `write`, `read` or both actions  	
- **Condition**: One or more JSON objects that enables scripting the effective condition, (see following section).	

And then save.	

To completely remove a Policy Template, click on the `DELETE POLICY` button.	

### Ladon conditions	

As already explained, Security Policies in Pydio Cells are built upon the [Ladon](https://github.com/ory/ladon) SDK. The smallest building block are `conditions` that are basically boolean assertions (e.g. `if` statements) scripted in JSON.	

Ladon comes with a few built-in conditions that are well explained in their [main README page](https://github.com/ory/ladon#conditions).	

For instance one of the simplest condition is the `StringMatchCondition` that simply returns `true` if the passed string matches a given pattern. For instance:	

```json	
{	
  "type": "StringMatchCondition",	
  "options": {	
    "matches": "localhost|127.0.0.1|::1"	
  }	
}	
``` 	

where `type` is the name of the condition and `options.matches` defines the corresponding regexp.	

For convenience, Pydio Cells adds some common and useful conditions:	

**StringNotMatchCondition**: simply checks if the passed string does **NOT** match this pattern.	

Syntax is the same as the ladon's built-in `StringMatchCondition` but will return the oposite:	

```json	
{	
  "type": "StringNotMatchCondition",	
  "options": {	
    "matches": "localhost|127.0.0.1|::1"	
  }	
}	
```	

Note the specific type that is String**Not**MatchCondition	

**OfficeHoursCondition**: returns true if current server time is within pre-defined day time period within the week and that can typically be used to define a policy that will prevent access to certain resources outside business times.	

For instance:	

```json	
{	
  "type": "OfficeHoursCondition",	
  "options": {	
    "matches": "Monday-Friday/09:00/18:30"	
  }	
}	
```	

Definition of valid days can be either defined using:	

- a single day: `Sunday`	
- interval like `Monday-Friday`	
- a list of days like `Monday,Tuesday,Friday` or `Monday, Tuesday, Friday`.	

Times are expressed with `HH:mm` format (hours are in 24 hours format) and are currently compared to current **server** time, **not the client time**. Thus, someone in Australia that connects on Monday 11AM Sydney's time to a **Pydio Cells** instance that is hosted in Berlin and that has such a policy won't be able to connect.	

**WithinPeriodCondition**: checks wether current server time was within the configured period at the time of the check.	

```json	
{	
  "type": "WithinPeriodCondition",	
  "options": {	
    "matches": "2018-02-01T00:00+0100/2018-04-01T00:00+0100"	
  }	
}	
```	

Date times used to define the boundaries of the period are expressed with an iso 8601 formatted string, e.g: "2006-01-02T15:04-0700".	

**DateAfterCondition**: returns true when checked if current server time is **after** configured date time.	

For instance:	

```json	
{	
  "type": "DateAfterCondition",	
  "options": {	
    "matches": "2018-02-28T23:59+0100"	
  }	
}	
```	

Where reference time is expressed with an iso 8601 formatted string.