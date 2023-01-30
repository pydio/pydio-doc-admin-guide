One of the first question of a system administrator before installing Cells is how to select a suitable setup. How to organize the services of Cells in an existing infrastructure while satisfying general requirements:

- Ensure the service availability
- Optimize resource usage
- Maintain at low cost

This article tries to figure out some typical use cases at different scales. It gives you some hardware configurations to start with. However, in the real life, 10 users with 10k uploaded files per days require more resources than a setup where 1000 users upload only 100 per day. To decide whether setup is a good fit for your case, you should prepare the answer for following questions:

- How many users do you have?
- Do you have a rough estimation of the number of existing files and **the growth rate of data over the time**?


Beside the usual "N-tier" architecture that recommends splitting storage from compute, Cells micro-service architecture allows each service to be independently deployable. They can run in either the same server or in different servers in the network.

## Architecture Samples

### A - All-In-One Server

This setup is suitable for a small organization with limited IT resource. All services and DB are installed in a single server. You can start quickly with this setup but over the time, system may reach limitations and the maintenance may become complex.

**Hardware configuration example**

- 4 cores CPU
- 8 G RAM
- Local disk storage

### B - Minimal Production Setup

Serving the database on a different server is always a good approach and Cells does not make exception on this. Some organizations maintain a central DB server for all application to optimize the cost of maintenance. With this setup, you can connect to an existing DB server, or install a personalized DB server for Cells.

**Hardware configuration example**

- Cells
  - 4 cores CPU
  - 8 G RAM
  - Local disk storage
- DB server (Sql + MongoDB)
  - 4 cores CPU
  - 4/8 G RAM
  

### C - Failover / Switchover Setup

This setup takes the advantage of standard backups of well-known services that you're already familial with (MySQL, etc.). In case of one node failure, you can quickly restore the service from the real-time backups. Depending on the acceptable downtime, this may be done manually (switchover) or automatically (failover). 

Additionally, running Cells in a dedicated server may allow you to isolate the server Cells in a DMZ network.

**Hardware configuration example**

- 01 server Cells
- 02 servers SQL in Master-Slave mode. For further information: https://mariadb.com/kb/en/replication-as-a-backup-solution/
- 02 servers MongoDB in Master-Slave mode
- 01 server S3 (minio). You can backup the data in S3 server by a standard file backup solution such as "rsync".

### D - Cluster High Availability (HA) Setup

Reaching full HA and zero-downtime can be done by deploying Cells in an auto-scalable, auto-healing cluster. This approach is detailed in the [Deploying Cells in a Distributed Environment](./deploying-cells-distributed-environment) section.

### Comparision between setups

|                                    | Availability                             | Scalability                              | Maintainability                          | Cost Efficiency                          |
|------------------------------------|------------------------------------------|------------------------------------------|------------------------------------------|------------------------------------------|
| A - Single server                  | &#9733; &#9733; &#9734; &#9734; &#9734;  | &#9734; &#9734; &#9734; &#9734; &#9734;  | &#9733; &#9733; &#9733; &#9733; &#9734;  | &#9733; &#9733; &#9733; &#9733; &#9733;  |
| B - Single server + Sql & Mongo DB | &#9733; &#9733; &#9734; &#9734; &#9734;  | &#9733; &#9733; &#9733; &#9734; &#9734;  | &#9733; &#9733; &#9733; &#9733; &#9733;  | &#9733; &#9733; &#9733; &#9733;  &#9734; |
| C - Switchover                     | &#9733; &#9733; &#9733; &#9733;  &#9734; | &#9733; &#9733; &#9733; &#9734;  &#9734; | &#9733; &#9733; &#9733; &#9733;  &#9734; | &#9733; &#9733; &#9733; &#9734;  &#9734; |
| D - High Availability              | &#9733; &#9733; &#9733; &#9733; &#9733;  | &#9733; &#9733; &#9733; &#9733; &#9733;  | &#9733; &#9733; &#9733; &#9734;  &#9734; | &#9733; &#9733; &#9734; &#9734;  &#9734; |

## Network Traffic

Also called "Network Flow" or "Network Diagram", the tables below summarize the necessary open ports for running Cells inside any infrastructure. You may communicate this page to your System Administrator if they ask you "what ports shall I open?"...

**Ingress**

|Source|Source Port|Protocol|Destination|Destination Port | Required | Comment
|---|---|---|---|---|---|---| 
|client IPs|Ephemeral port range|TCP|Cells IP|443|yes|https & http/2|
|client IPs|Ephemeral port range|TCP|Cells IP|80||http redirection|
|client IPs|Ephemeral port range|TCP|Cells IP|22||ssh|
|client IPs|Ephemeral port range|TCP|Cells IP|2022 (optional)||sftp service|

**Egress**

|Source|Source Port|Protocol|Destination|Destination Port | Required | Comment
|---|---|---|---|---|---|---| 
|Cells IP|Ephemeral port range|UDP|y.y.y.y|123|yes|ntp/chrony for time synchronization|
|Cells IP|Ephemeral port range|TCP|updatecells.pydio.com|443||Update cells service|
|Cells IP|Ephemeral port range|TCP/UDP|DNS IPs|53||Dns service|
|Cells IP|Ephemeral port range|TCP|MySQL DB|3306||MySQL DB |
|Optional Services|
|Cells IP|Ephemeral port range|TCP|database.clamav.net|80|| freshclam for antivirus service
|Cells IP|Ephemeral port range|TCP|sso.server.com|443||sso server such as saml, openid connect, adfs| 
|Cells IP|Ephemeral port range|TCP|smtp server|25/587/465||SMTP server
|Cells IP|Ephemeral port range|TCP|Ldap server IP|389/636/3268|| Ldap server
|Cells IP|Ephemeral port range|TCP|S3 server IP|443|| S3 service object


OS specific ephemeral port range: https://en.wikipedia.org/wiki/Ephemeral_port

For further information, please visit: https://share.pydio.com/public/5bbf4953a262