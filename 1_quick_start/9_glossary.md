This section provides definitions for commonly used terms of this documentation.


| Term  | Definition  |
|---|---|
|cells| Static binary, compiled for each platform. `cells` contains **all microservices** necessary to launch the server platform, as well as a couple of command-line tools to communicate with a running Cells instance. |
|cells-enterprise| Static binary containing `cells` and additional microservices for implementing Cells Connect and Cells Enterprise features. Requires a valid license key to run. |
|cells-sync| Desktop synchronization client. `cells-sync` communicates with a running Cells server. |
|cells-client| Command line client that provides tools to communicate with a running Cells server via its REST API. |
|FQDN| Fully Qualified Domain Name, e.g. _files.example.com_ |
|Gateway | Receives all incoming HTTP/TCP connections to Cells and acts as a load balancer / proxy / routing service to the multiple microservices of the Cells environment. |
|Microservice | Independant brick that exposes a set of internal APIs inside Cells. For example, there are microservices for managing users, workspaces, logs, etc.   |
|Site | Configuration item describing an entrypoint to the Gateway. |
|Binding Address  | [ip:port] declared by a Site to bind to a given port of a network interface.|
|External URL | Users access this URL to connect to Cells. It can be exposed by a third-party Reverse Proxy. It must be fully defined, such as http[s]://&lt;ip&#124;domain&gt;[:port] . |
|Datasource | Cells access to a physical storage, can be seen as a "mount point" to either a local filesystem, a S3-compatible storage, etc. Cells indexes all content detected in all datasources. |
|Workspace | Virtual access to any file/folder indexed in any datasource. Workspaces are managed by Administrators. |
|Cell | Virtual access granted by users to others on the files they are allowed to see. Cells inherit their parent Workspace security rules, and can be seen as "sandboxed" user-managed workspaces. |
|Role-Based ACL | Permissions inheritance system based on roles and groups  |
|Rule-Based ACL | Permissions inheritance system based on Security Policies  | 
|Role | Set of permissions that can be applied to any user.  | 
|Group | Group of users, coming with their own set of permissions. Users can only belong to one group and inherit their parent groups permissions. |   
|Security Policy | Condition-based rules dynamically computed to filter listings and accesses. Matched against the user/query/metadata context. |  
|Versioning Policy | Admin-defined rules for keeping/removing/pruning files versions in each datasource.  | 
