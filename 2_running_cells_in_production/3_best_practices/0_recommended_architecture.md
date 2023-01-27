One of the first question of system admin before installing Cells is how to select a suitable setup. How to organize the services of Cells in an existed infrastructure that satisfies general requirements:
- Assure the availability of service
- Optimized resource usage
- Low cost of maintenance

This article tries to figure out some typical use cases at different scales. It gives you some hardware configurations to start with. However, in the real life, 10 users with 10k uploaded files per days require more resources than a setup where 1000 users with only 100 upload files. To decide whether setup is a good fit for your case, you should prepare the answer for following questions:
- How many user you have?
- Estimated of number of existed files and the growth rate of data over the time?


Cells was designed and developed based on microservice architecture. This architectural style allows Cells be installed as a collection of services. Each service is independently deployable. They can run in either the same server or in different servers in the network.


## All-In-One Server
This setup is suitable for a small organization with limited IT resource. All services and DB are installed in a single server. You can go quickly with this setup but over the time, the maintenance become complicate.

**Hardware configuration example**
- 4 cores CPU
- 8 G RAM
- Local disk storage

## Minimal Production Setup
Keeping the database out of application server is always a good approach and Cells is not an exceptional case. Some organizations maintain a central DB server for all application to optimize the cost of maintenance. With this setup, you can profit the existed DB server or install a personalized DB server for Cells.

**Hardware configuration example**

- Cells
  - 4 cores CPU
  - 8 G RAM
  - Local disk storage
- DB server (Sql + MongoDB)
  - 4 cores CPU
  - 4/8 G RAM
  

## Failover/Switchover Setup

This setup takes the advantage of standard backup of well-known productions that you're already familial with. In case of failure of one node, you can quickly restore the service from the real-time backups. 

Additionally, running Cells in a dedicated server, you can isolate the server Cells in a DMZ network 

**Hardware configuration example**
- 01 server Cells
- 02 servers SQL in Master-Slave mode. For further information: https://mariadb.com/kb/en/replication-as-a-backup-solution/
- 02 servers MongoDB in Master-Slave mode
- 01 server S3 (minio). You can backup the data in S3 server by a standard file backup solution such as "rsync".
  

## Full Cluster HA Setup

For further information of HA setup, please visit: https://pydio.com/en/docs/cells/v4/going-stateless


**Comparision between different setups**

| Setup    | availability  | scalability  | maintainability | cost efficiency |
|---|---|---|---|---|
| Single server  | █ █ ░ ░ ░  | ░ ░ ░ ░ ░  | █ █ █ █ ░  | █ █ █ █ █  |
| Single server + dedicated Sql DB  | █ █ ░ ░ ░  | █ █ █ ░ ░  | █ █ █ █ █  | █ █ █ █ ░ |
| Switchover   | █ █ █ █ ░ | █ █ █ ░ ░ | █ █ █ █ ░  | █ █ █ ░ ░ |
| High Availability   | █ █ █ █ █  | █ █ █ █ █  | █ █ █ ░ ░  | █ █ ░ ░ ░ |