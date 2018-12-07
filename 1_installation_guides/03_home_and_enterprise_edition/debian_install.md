
_This guide describes the steps required to have Pydio Cells running on Debian 8 and Debian 9_.

[:image-popup:1_installation_guides/logos-os/logo-debian.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Debian or Raspbian versions:

- Stretch 9 (stable) / Raspbian Stretch
- Jessie 8 (LTS) / Raspbian Jessie

#### Dedicated User

It's highly recommend to run Pydio Cells with a dedicated user.

In this guide, we use **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
```

### Database

Pydio Cells can be installed with both MySQL Server (v5.6 or higher) and MariaDB (v10.2 or higher).

#### MySQL

##### Repository

```bash
wget https://repo.mysql.com/mysql-apt-config_0.8.9-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.9-1_all.deb
```

To access the correct repository :

- Select the `MySQL Server & Cluster` option and press `Enter`
- Select the correct option (`mysql-5.6` or `mysql-5.7`) and press `Enter`
- Back on the first list, select `Ok` and press `Enter`

##### Install

```bash
sudo apt update
sudo apt install mysql-server
```

#### MariaDB

##### Repositories

We currently use MariaDB 10.3, here is the [official installation guide on the MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=CentOS&version=10.3&mirror=23Media&distro_release=centos7-ppc64--centos7).

Simply enter there your system specifications and follow the detailed instructions.

#### Post install configuration

By default, a new database will be created by the system during the installation process. You only need a user with database management permissions.

If you would rather do it manually, you may create a dedicated user and an empty database by executing the following SQL queries :

```SQL
CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<your-password-here>';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
FLUSH PRIVILEGES;
```

## Installation and configuration

```sh
wget https://download.pydio.com/pub/cells/release/1.2.3/linux-amd64/cells
sudo chmod u+x cells
```

If you need to use the standard http (80) or https (443) port, please execute this command:

```sh
setcap 'cap_net_bind_service=+ep' cells
```

Switch to the **pydio** user to run the installation and start the app:

```sh
su - pydio
```

Execute the command below and follow the instructions.

**Before you start installing here's two of the most important parameters that you need to understand:**
```
CELLS_BIND : address where the application http server is bound to. It MUST contain a server name and a port.
CELLS_EXTERNAL : url the end user will use to connect to the application.
Example:
If you want your application to run on the localhost at port 8080 and use the url mycells.mypydio.com, then set CELLS_BIND to localhost:8080 and CELLS_EXTERNAL to mycells.mypydio.com
After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:
```

```sh
./cells install
```

You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details on the installation process.

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

```sh
./cells start
```

## Troubleshooting

Generally, you might want to have a look at the log file that is located in `~/.config/pydio/cells/logs`.

### Database server issue

_After start, the web page is unreachable and you see a bunch of errors starting with: `ERROR   pydio.grpc.meta   Failed to init DB provider   {"error": "Error 1071: Specified key was too long; max key length is 767 bytes handling data_meta_0.1.sql"}`_.

You might have an unsupported version of the mysql server: you should use MySQL server version 5.6 or higher or MariaDB version 10.2 or higher.

### Various

_You see this error: `/lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.14' not found`_

The version of libc6 is outdated. Run these commands to upgrade it.

```sh
sudo apt-get update
sudo apt-get install libc6
```
