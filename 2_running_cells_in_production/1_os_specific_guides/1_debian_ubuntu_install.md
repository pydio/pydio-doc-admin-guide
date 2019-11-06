
_This guide describes the steps required to have Pydio Cells running on Debian and Ubuntu_.

[:image-popup:2_running_cells_in_production/logos-os/logo-debian.png]
[:image-popup:2_running_cells_in_production/logos-os/logo-ubuntu.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Debian or Ubuntu versions:

- Debian 10 (buster) LTS 
- Stretch 9 (stable) / Raspbian Stretch
- Jessie 8 (LTS) / Raspbian Jessie
- Ubuntu 18.04 (bionic beaver)
- Ubuntu 16.04 (xenial xerus)

#### Dedicated User

It's highly recommended to run Pydio Cells on a dedicated user.

In this guide, we use a dedidcated user **pydio** and its home directory **/home/pydio**.

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

```bash
sudo apt update
sudo apt install mysql-server
```

#### MariaDB

##### Repositories

_If you are using Debian 10 or Ubuntu 18.04 you can skip this step, MariaDB 10.3 is in the default packages_

We currently use MariaDB 10.4, here is the [official installation guide on the MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=Debian&version=10.4).

Simply enter there your system specifications and follow the detailed instructions.

In production system, you probably want to secure your installation by executing:

`mysql_secure_installation`

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
# Use this url as is, will be redirected to latest version automatically
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

**Before you start installing here's two of the most important parameters that you need to understand:**

```sh
INTERNAL_URL(other) : address where the application http server is bound to. It MUST contain a server name and a port, should be of this form <ip-or-domain>:<port>.

EXTERNAL_URL : Is the entrypoint for the webui, should be modified only for specific cases such as a Reverse Proxy.

Example:
If you want your application to run on the localhost at port 8080 and use the url mycells.mypydio.com, then set INTERNAL_URL to localhost:8080 and EXTERNAL_URL to http://mycells.mypydio.com (or https)
```

You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details on the installation process.

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
