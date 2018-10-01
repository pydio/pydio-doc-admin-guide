_Pydio Cells is a full rewrite of the Pydio project using future-proof technologies. After hitting hard-limits of PHP language, the core team decided to swith to Go, an open source server language written and maintained by Google._

## End-Users: ultimate sharing experience

### Cells

Users can now create their own collaboration spaces, based on a selection of files or folders, a group of users with whom they want to work, or any project-specific topic. These cells can be seen as dynamic workspaces managed by the users and inheriting the security properties (ACLs, security policies, etc...) from the admin-defined workspaces.

Once created, Cells will appear to the users in the left column, and their contents and collaborators can be easily edited. Each cell also provide a dedicated real-time messaging space, and in fact users of popular group-chat application will find the concept very close of a discussion channel, but focused on files.

### Address Book sharing

To go further with collaboration, user-defined teams and users can be shared with other users as well, either with read or write properties. This "Visibility" concept is also applied to Cells and Public Links, providing ultimate flexibility to end-users for their day-to-day work.

### Versioning

Cells directly embeds a versioning mechanism to allow end-users to revert documents to any previous version with ease.

Versioning policies define retention periods (how many versions of a file are kept for a given time) and can be applied on a per-datasource basis (see below). The Home Edition comes with a set of predefined policies, whereas the Enterprise Edition allows admin to create their own policies.

## Administrators: security and compliance

### Workspaces vs. Datasources

In Pydio 8, Workspaces were defining both a way to access the actual storage (e.g. FileSystem, Samba, S3, etc...), and the atomic level for assigning rights to the end-users. This is now decoupled and storages are now defined by "DataSources", whereas Workspaces will now be defined by pointing to one or more path of any datasource. Splitting the atom between Logical and Technical aspect provides much more flexibility and allows a better implementation of many features.

### Security Policies

Resource-based security policies are similar to Amazon Resource-Based policies. This flexible concept allows creation of complex rules based on many parameters like request IP address, server time, files metadata, etc. These rules can then be applied on the following resources:

- REST Access points (can be applied to one or many HTTP Method)
- OpenID Connect Access Point (to allow/disallow login operations)
- [ED] ACLs: using policies instead of simple Read/Write assignments

See the dedicated [chapter on security](/en/docs/cells/v1/security-policies): Home Edition comes with a generic set of policies to secure the installation, whereas Enterprise Edition allows fine tuning and creation of bespoke policies.

### [ED] Auditable Logs

In the Enterprise Edition, GDPR-compliant logs are provided by splitting system logs from business events and exporting those toward a dedicated service. These Audit logs can be searched, filtered and exported to XLSX or CSV format.

## DevOps: micro-services architecture

The technology shift was also the opportunity to actually _"break the monolith"_. Pydio Cells is a set of micro-services that can be distributed of different physical or virtual machines on a network, allowing greater flexibility, interfacing and scaling.

You can get an overview on the services from your admin dashboard by looking in **Backend > Services**.
