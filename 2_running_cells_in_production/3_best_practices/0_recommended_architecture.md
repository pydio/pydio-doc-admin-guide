Recommended Architectures for Cells.

One of the first question of system engineer before install Cells is how to select a suitable setup. How to organize the services of Cells in an existed infrastructure that satisfies following requirements:
- Assure the availability of service
- Optimized resource usage
- Low cost of maintenance

We hope that this article will help you decide whether setup is a good fit for your case.

Cells was designed and developed based on microservice architecture. This architectural style allows Cells be installed as a collection of services. Each service is independently deployable. They can run in either the same server or in different servers in the network.


### Cells in a single server
This setup is suitable for a small organization with limited IT resource

#### Hardware requirements
- 4 cores CPU
- 8 G RAM
- Local disk storage

### Simple setup - Cells in a server and separated DB servers
During the development and testing, we recognized that the Sql db is one of hot spots. We highly recommend this setup in both small and middle size organization.

#### Hardware requirements
##### Cells
- 4 cores CPU
- 8 G RAM
- Local disk storage
##### DB server (Sql + MongoDB)
- 4 cores CPU
- 4/8 G RAM
  

### Switchover setup
This setup takes the advantage of standard backup of well-known productions that you're already familial with. In case of failure of one node, you can quickly restore the service from the real-time backups. This setup requires several nodes:

#### Hardware requirements
- 01 server Cells
- 02 servers SQL in Master-Slave mode. For further information: https://mariadb.com/kb/en/replication-as-a-backup-solution/
- 02 servers MongoDB in Master-Slave mode
- 01 server S3 (minio). You can backup the data in S3 server by a standard file backup solution such as "rsync".
  

### High Available (HA) setup

For further information of HA setup, please visit: https://pydio.com/en/docs/cells/v4/going-stateless


| Setup    | availability  | scalability  | maintainability | cost efficiency |
|---|---|---|---|---|
| Single server  | █ █ ░ ░ ░  | ░ ░ ░ ░ ░  | █ █ █ █ ░  | █ █ █ █ █  |
| Single server + dedicated Sql DB  | █ █ ░ ░ ░  | █ █ █ ░ ░  | █ █ █ █ █  | █ █ █ █ ░ |
| Switchover   | █ █ █ █ ░ | █ █ █ ░ ░ | █ █ █ █ ░  | █ █ █ ░ ░ |
| High Availability   | █ █ █ █ █  | █ █ █ █ █  | █ █ █ ░ ░  | █ █ ░ ░ ░ |