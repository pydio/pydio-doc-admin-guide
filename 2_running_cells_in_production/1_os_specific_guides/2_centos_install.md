_This guide describes the steps required to have Pydio Cells running on a CentOS/RHEL 7 or 8 server._

[:image:2_running_cells_in_production/logos-os/logo-centos.png]

## Prerequisites

### Database

The only hard requirement is a running MySQL database server. We recommend using MariaDB, version 10.4.

#### MariaDB

We currently use MariaDB 10.4, here is the [official installation guide on the MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=CentOS&version=10.4&distro_release=centos8-amd64--centos8).

Double check that the system specifications are OK and follow the detailed instructions.

After installation, you should enable and start the service:

```sh
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

#### MySQL

Install MySQL official community release repository.

```bash
sudo rpm -i http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
sudo yum update

# install mysql-community-server package
sudo yum install mysql-community-server

# Set mysqld to start after reboot
sudo systemctl enable mysqld

# start the service now
sudo systemctl start mysqld
```

#### Post install configuration

By default, a new database is created by the system during the installation process. You only need a user with database management permissions.

If you would rather do it manually, you may create a dedicated user and an empty database by executing the following SQL queries :

```SQL
CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<your-password-here>';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
FLUSH PRIVILEGES;
```

### SELinux

There is no available configuration of SELinux for Pydio Cells. Please make sure that SELinux is disabled or running in permissive mode.

To temporary disable SELinux: `sudo setenforce 0`.

You can also permanently disable SELinux in `/etc/selinux/config`.

### Dedicated User

It is recommended to use a dedicated non-admin user to run Pydio Cells.

In this guide, we use **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
sudo passwd pydio
```

To switch to this user:

```sh
su - pydio
```

## Install Pydio Cells

```sh
# As pydio user, downlaod the latest version
wget https://download.pydio.com/latest/cells/release/{latest}/linux-amd64/cells
chmod u+x cells
```

If you want to directly bind Cells to the standard HTTP (80) and/or HTTPS (443) ports, you have to give corresponding permissions to the binary file:

```sh
sudo setcap 'cap_net_bind_service=+ep' /home/pydio/cells
```

**Before you start installing, there are two important parameters that you need to understand:**

- **Internal URL**: it defines the interface where the internal webserver of the application is bound. It MUST contain a server name and a port and must be formatted this way: `<ip-or-domain>:<port>`.

- **External URL**: This is the main entry point from the outside world; the address you will communicate to your end-users. It typically  differs from the internal URL when you are behind a reverse proxy or in a container.

For instance, your application runs in a VM that has this IP: 10.0.0.2 in a private LAN behind a reverse proxy that has a public IP and a A DNS record for domain cells.example.com.
Then set INTERNAL_URL to 10.0.0.2:8080 and EXTERNAL_URL to https://cells.example.com (or http).

You can now run the installer:

```sh
# As pydio user
./cells install
```

Follow the short set of instructions. You can also [refer to this page](./cells-installation) to get more details.  
When the installation is done, you might have to stop and restart the application (typically if you have chosen the CLI installer).

```sh
./cells start
```

Note that this is not the preferred way to run Pydio Cells in a production context, see [the following chapters of our documentation](./run-cells-service) to fine tune a production ready instance.

## Troubleshooting

### SELinux is enforced

If, after a successful installation and when you try to navigate to the main application page with your browser, you land on a blank page with following message:

> Access denied.

ensure you have modified SELinux to be in permissive mode.
