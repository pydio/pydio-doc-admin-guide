
_This guide describes the steps required to have Pydio Cells running on Debian and Ubuntu_.

[:image:2_running_cells_in_production/logos-os/logo-debian.png]


[:image:2_running_cells_in_production/logos-os/logo-ubuntu.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Debian or Ubuntu versions:

- Debian 10 (buster) LTS
- Stretch 9 (stable) / Raspbian Stretch
- Jessie 8 (LTS) / Raspbian Jessie
- Ubuntu 18.04 (Bionic Beaver)
- Ubuntu 16.04 (Xenial Xerus)

#### Dedicated User

It is highly recommended to run Pydio Cells with a dedicated user.

In this guide, we use a dedicated user **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
```

### Database

Pydio Cells can be installed with both MySQL Server (5.7 or higher) or MariaDB (10.3 or higher).

#### MySQL

##### Repository

```bash
wget https://repo.mysql.com/mysql-apt-config_0.8.9-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.9-1_all.deb
```

To access the correct repository :

- Select the `MySQL Server & Cluster` option and press `Enter`
- Select the correct option (`mysql-5.7` or `mysql-8`) and press `Enter`
- Back on the first list, select `Ok` and press `Enter`

##### Install

```sh
sudo apt update
sudo apt install mysql-server
```

#### MariaDB

We advise to use MariaDB 10.4.  
Please refer to the well maintained [official installation guide on the MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=Debian&version=10.4).

> If you plan going live, you probably want to execute this convenient script to reduce your attack surface:  
> `mysql_secure_installation`

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
# Use this url as is, you will be redirected to latest version automatically
wget https://download.pydio.com/latest/cells/release/{latest}/linux-amd64/cells
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

**Before you start installing, here are two important parameters that you need to understand:**

- Internal URL: it defines the interface where the internal webserver of the application is bound. It MUST contain a server name and a port, should be of this form <ip-or-domain>:<port>.

- External URL: This is the main entry point from the outside world, the address you will communicate to your endusers. It typically  differs from the internal URL when you are behind a reverse proxy or in a container.

For instance, you application runs in a VM that has this IP: 10.0.0.2 in a private LAN behind a reverse proxy that has a public IP and a A DNS record for domain cells.example.com.
Then set INTERNAL_URL to 10.0.0.2:8080 and EXTERNAL_URL to https://cells.example.com (or http).

You can [refer to this page](en/docs/cells/v2/cells-installation) to get more details on the installation process.

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

```sh
./cells start
```

**It is advised to add Cells as a service with systemd (or supervisor) - See your Knowledge Base for the dedicated Guides.**

## Troubleshooting

Generally, you might want to have a look at the log file that is located in `~/.config/pydio/cells/logs`.

### Database server issue

_After start, the web page is unreachable and you see a bunch of errors starting with: `ERROR   pydio.grpc.meta   Failed to init DB provider   {"error": "Error 1071: Specified key was too long; max key length is 767 bytes handling data_meta_0.1.sql"}`_.

You might have an unsupported version of the mysql server: you should use MySQL server version 5.7 or higher or MariaDB version 10.3 or higher.

### Various

_You see this error: `/lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.14' not found`_

The version of libc6 is outdated. Run these commands to upgrade it.

```sh
sudo apt-get update
sudo apt-get install libc6
```
