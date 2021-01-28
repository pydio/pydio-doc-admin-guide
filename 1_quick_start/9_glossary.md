This section provides definitions for commonly used terms of this documentation.


| Term  | Definition  | See also  |
|---|---|---|
|cells| Static binary, precompiled for each platform. `cells` contains **all micro-services** necessary to launch the server platform, as well as a couple of command-line tools to communicate with a running Cells instance. | |
|cells-enterprise| Static binary containing every `cells` services plus additional ones for implementing Cells Connect and Cells Enterprise features. Requires a valid license key to run. | |
|cells-sync| **Desktop** synchronization client. `cells-sync` communicates with a running Cells server. | |
|cells-client| Command line client that provide tools to communicate with a running Cells server via its REST API. | |
|Service | A service, or micro-service, is an independant brick that exposes a set of internal APIs inside Cells. For example, there are services for managing users, maganing workspaces, managing logs, etc.   | [Cells Internals](./pydio-cells-internals)  |
|Gateway | Receives all incoming HTTP/TCP connections to Cells and acts as a load balancer / proxy / routing service to the multiple microservices of the Cells environment |   |
|Site | Configuration item describing an entrypoint to the Gateway | |
|Binding Address  | [ip:port] declared by a Site to bind to a network interface, as would a classical http server do.|   |
|External URL | Full URL http[s]://&lt;ip&#124;domain&gt;[:port] |   |
|FQDN|||
|Datasource |   |   |
|Workspace |   |   |
|Role |   |   |
|Group |   |   |
|Security Policy |   |   |
|Versioning Policy |   |   |
