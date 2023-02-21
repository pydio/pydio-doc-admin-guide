Cells ships as a set of precompiled static binaries, one for each operating system. The only required dependency is a MySQL database.

## Hardware/OS

- **CPU**: AMD 64 bits, ARM 32 or 64 bits. 2 core or more are recommended.
- **RAM**: 4GB RAM
- **Disk**: SSD is highly recommended for storage.
- **Supported OS**:
  - _Debian Like_: Debian 11 (Bullseye), 10 (Buster), 9 (Stretch); Raspbian 10 or 11; Ubuntu 22.04 (Jammy Jellyfish), 20.04 (Focal Fossa), 18.04 (Bionic Beaver)
  - _RHEL_: RHEL 7, 8 or 9, RockyLinux 8 or 9, CentOS 7
  - _MacOSX_: from 10.13
  - _Windows_: 10 or 11 (Cells Home only)

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

This can generally be done with a similar instruction : 
```SQL
mysql -u root
SET GLOBAL max_connections = 1000;
SHOW VARIABLES LIKE "max_connections";

SET GLOBAL max_prepared_stmt_count = 131056;
SHOW VARIABLES LIKE "max_prepared_stmt_count";
```

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

Learn more about the [reasons to provide a MongoDB here](./configuring-mongo-storage).

**Supported Mongo Versions:** MongoDB has currently been tested with v5+

## Network requirements

### Gateway
Cells provides its own webserver as a unified Gateway to public-facing services. As such, unlike old versions of Pydio, you **do not** need Apache or equivalent.

The Gateway will "bind" to a TCP Port like a standard web server. The port number used is defined in the ./cells configure sites command, or directly as a parameter of the start command in the command line interface.

_This is the port that needs to be opened in your firewall to make Cells accessible outside the server._

| Port    | Default | CLI Flag |
|---------|---------|----------|
| Gateway | 8080    | --bind   |

### Services
Cells uses TCP/IP connections to communicate between services. The following are using pre-defined ports, that can be overridden in all commands that refer to services in the command line interface (eg start, admin, ...).

| Port               | Default | CLI Flag              |
|--------------------|---------|-----------------------|
| Discovery Services | 8030    | --grpc_discovery_port |
| gRPC Server Port   | 8031    | --grpc_port           |

## Other requirements

The date & time on the server **must** be correct, otherwise you will have authentication problems; e.g: `403` access forbidden errors when trying to upload.

If you plan to use one of the clients (e.g.: Cells Sync, the mobile apps, Cells clients), you must also at least define the public external URL. Otherwise, Cells cannot complete the OAuth credential flow.
