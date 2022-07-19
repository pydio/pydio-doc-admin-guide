Cells ships as a set of precompiled static binaries, one for each operating system. The only required dependency is a MySQL database.

## Hardware/OS

- **CPU**: AMD 64bit architecture only, 2 core or more are recommended.
- **RAM**: 4GB RAM
- **Disk**: SSD is highly recommended for storage.
- **Supported OS**:
  - _Debian Like_: Debian 10 (Buster) LTS, Debian 9 (Stretch) LTS / Raspbian Stretch, Debian 8 (Jessie) LTS / Raspbian Jessie, Ubuntu 20.04 (Focal Fossa), Ubuntu 18.04 (Bionic Beaver), Ubuntu 16.04 (Xenial Xerus)
  - _RHEL_: RHEL7, CentOS7,  RHEL6, CentOS 6 
  - _MacOSX_: 10.13/11.1
  - _Windows_: 10 (Cells Home only)

**Important Settings**

- **Dedicated OS user**: never run Cells as "root" user!
- **Ulimit**: the number of allowed open files must be greater than **2048**. For production use, a minimum of **8192** is recommended (see `ulimit -n`).

## MySQL/Maria DB versions

Supported server versions:

- [MariaDB version 10.3 and above](https://downloads.mariadb.org/mariadb/repositories)
- [MySQL version 5.7 and above](https://dev.mysql.com/doc/refman/8.0/en/installing.html) (_**except 8.0.22** that has a bug preventing cells to run correctly_)

**Creating a database and a privileged user**

```SQL
CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<your-password-here>';
CREATE DATABASE cells DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
FLUSH PRIVILEGES;
```

**Important Settings** 

Always ensure that the database is created with character set and collation set to UTF8.

Make sure not to leave the `max_connections` to its default value (151) for production we advise at least `500`, for a better understanding [see MySQL manual](https://dev.mysql.com/doc/refman/8.0/en/too-many-connections.html).

## [Optional] Mongo DB

Depending on your target deployment size, you may switch directly from the default on-file storage used by specific services (search engine, activity feeds, logs, amongst others) to a Mongo DB document store. 

This is a good idea if  

- You foresee a high load on the platform:  number of users and/or a high number of files managed every day.   
- You plan to deploy Cells in a distributed environment (cluster) to provide high availability or horizontal scaling.  

If you do not have a Mongo DB available yet, **you can add it later** as Cells provides the tools to migrate from on-file storage to Mongo. Otherwise, prepare Mongo connection information such as : 

| Parameter                    | Description                                                                          | Defaults                  |
|------------------------------|--------------------------------------------------------------------------------------|---------------------------|
| Host/Port                    | Address of the server hosting the Mongo DB                                           | localhost:27317           |
| Credentials                  | Optional credentials to connect, along with an authentication DB name                | [none]                    |
| Database Name                | Name of the mongo database                                                           | cells                     |
| Connection String Parameters | Additional connection parameters passed via Mongo connection string query parameters | maxPoolSize=20&w=majority |

Learn more about the [reasons to provide a MongoDB here](/configuring-mongo-storage).

## Network requirements

### Gateway
Cells provides its own webserver as a unified Gateway to public-facing services. As such, unlike old versions of Pydio, you **do not** need Apache or equivalent.

The Gateway will "bind" to a TCP Port like a standard web server. The port number used is defined in the ./cells configure sites command, or directly as a parameter of the start command in the command line interface.

_This is the port that need to be opened in your firewall to make Cells accessible outside the server._

| Port    | Default   | CLI Flag |
| ------- | --------- | -------- |
| Gateway | 8080      | --bind

### Services
Cells uses TCP/IP connections to communicate between services. The following are using pre-defined ports, that can be overridden in all commands that refer to services in the command line interface (eg start, admin, ...)

| Port               | Default | CLI Flag        |
|--------------------|---------|-----------------|
| gRPC Server Port   | 8001    | --grpc_port     |
| Discovery Services | 8002    | --grpc_discovery_port |