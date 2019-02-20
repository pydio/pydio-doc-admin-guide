_This guide describes the steps required to have Pydio Cells running on a CentOs/RHEL 7 server._

[:image-popup:1_installation_guides/logos-os/logo-centos.png]

## Prerequisites 

### Database

The only hard requirement is a running MySQL DB server. We recommand a recent version of MariaDB or the MySQL community server.

#### MariaDB

We currently use MariaDB 10.3, here is the [official installation guide on the MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=CentOS&version=10.3&mirror=23Media&distro_release=centos7-ppc64--centos7).

Simply enter there your system specifications and follow the detailed instructions.

#### MySQL

Install MySQL 5.6 official community release repository.

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

By default, a new database will be created by the system during the installation process. You only need a user with database management permissions.

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

It is recommended to use a dedicated user to run Pydio Cells.

In this guide, we use **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
sudo passwd pydio
```

Switch to this user to run the installation

```sh
su - pydio
```

## Install Pydio Cells

```sh
wget https://download.pydio.com/pub/cells/release/1.4.0/linux-amd64/cells
chmod u+x cells
# if you need to use the standard http (80) or https (443) port, please execute this command:
setcap 'cap_net_bind_service=+ep' cells
./cells install
```

**Before you start installing here's two of the most important parameters that you need to understand:**
```
CELLS_BIND : address where the application http server is bound to. It MUST contain a server name and a port.
CELLS_EXTERNAL : url the end user will use to connect to the application.
Example:
If you want your application to run on the localhost at port 8080 and use the url mycells.mypydio.com, then set CELLS_BIND to localhost:8080 and CELLS_EXTERNAL to mycells.mypydio.com
After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:
```

Follow the short set of instructions to finish off the Pydio Cells installation. You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details.

```sh
./cells start
```

## Troubleshooting

### Frontend

#### SELinux is enforced

If, after a successful installation and when you try to navigate to the main application page with your browser, you land on a blank page with following message:

> Access denied.

Insure you have modified SELinux to be in permissive mode.
