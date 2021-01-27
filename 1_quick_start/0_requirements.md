## Hardware/OS

For Pydio Cells to run smoothly, you should meet the following requirements:

- **Server Capacity** : 2 Core CPU - 64bit, 4GB RAM, SSD is also recommended for storage.
- **Operating System**: Debian 8/9/10, Ubuntu 18.04 LTS, CentOS 7, MacOS 10.13/11.1, Windows 10
- **Ulimit**: Make sure to set the number of allowed open files greater than **2048**. For production use, a minimum of **8192** is recommended (see `ulimit -n`).

## MySQL Database

You must have an available MySQL database, along with a privileged user (e.g. `pydio`) to access this DB. Use one of the following link to install the DB:

- [MariaDB version 10.3 and above](https://downloads.mariadb.org/mariadb/repositories)
- [MySQL version 5.7 and above](https://dev.mysql.com/doc/refman/8.0/en/installing.html) (_**except 8.0.22** that has a bug preventing cells to run correctly_).

As recommended by databases documentations, make sure not to leave the `max_connections` to its default value (151) while going live in production.

