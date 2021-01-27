Cells ships as a set of precompiled static binaries, one for each operating system. The only required dependency is a MySQL database.

## Hardware/OS

- **CPU**: 64bit architecture only, AMD/ARM, 2 Core or more are recommended 
- **RAM**: 4GB RAM 
- **Disk**: SSD is highly recommended for storage.
- **Supported OS**: 
  + _Debian Like_: Debian 10 (Buster) LTS, Debian 9 (Stretch) LTS / Raspbian Stretch, Debian 8 (Jessie) LTS / Raspbian Jessie, Ubuntu 20.04 (Focal Fossa), Ubuntu 18.04 (Bionic Beaver), Ubuntu 16.04 (Xenial Xerus)
  + _RHEL_: RHEL6/CentOS 6 
  + _MacOSX_: 10.13/11.1 
  + _Windows_: 10 (Cells Home only)
    
**Important Settings**

 - **Dedicated OS user**: never run Cells as "root" user!
 - **Ulimit**: the number of allowed open files must be greater than **2048**. For production use, a minimum of **8192** is recommended (see `ulimit -n`).


## MySQL/Maria DB versions

Supported server versions:

 + [MariaDB version 10.3 and above](https://downloads.mariadb.org/mariadb/repositories)
 + [MySQL version 5.7 and above](https://dev.mysql.com/doc/refman/8.0/en/installing.html) (_**except 8.0.22** that has a bug preventing cells to run correctly_)

**Creating a database and a privileged user**

```SQL
CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<your-password-here>';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
FLUSH PRIVILEGES;
```


**Important Settings** 

Make sure not to leave the `max_connections` to its default value (151) while going live in production.


## Embedded webserver

Cells microservice architecture provides its own webserver to provide a unified Gateway to the underlying services. 
As such, unlike old versions of Pydio, you **do not** need Apache or equivalent.

Providing web access to the platform, this Gateway will need to "bind" itself to an available TCP port like a standard
web server (typically 80/443, or any other port). 