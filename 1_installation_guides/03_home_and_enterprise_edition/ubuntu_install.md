_This guide describes the steps required to have Pydio Cells running on Ubuntu._

[:image-popup:1_installation_guides/logos-os/logo-ubuntu.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Ubuntu version:

- Ubuntu 18.04 LTS (Bionic Beaver)
- Ubuntu 16.04 LTS (Xenial Xerus)

#### Dedicated User

It's highly recommend to run Pydio Cells with a dedicated user.  
In this guide, we use **pydio** and its home directory **/home/pydio**.  
In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m -s /bin/bash pydio
sudo passwd pydio
# to ease later manipulation, you might also add your current user to the pydio group
sudo usermod -aG pydio <youruser>
sudo chmod -R g+w /home/pydio
# log out and back in for the group modification to be taken into account
```

_Note: the `-s /bin/bash` option is not strictly required. It insures you are using bash shell when logged in with this user and have, among others, access to bash history feature_.

### Database

Pydio Cells can be installed with both MySQL Server (v5.6 or higher) and MariaDB (v10.3 or higher).

#### MariaDB server

We currently use MariaDB 10.3, here is the [official installation guide on the MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=Ubuntu&version=10.3).

Simply enter there your system specifications and follow the detailed instructions.

#### MySQL server

```sh
# On Ubuntu 18.04
sudo apt-get install mysql-server-5.7
# On Ubuntu 16.04
sudo apt-get install mysql-server-5.6
```

#### Configuration

By default, a new database will be created by the system during the installation process. You only need a user with database management permissions.

If you would rather do it manually, you may create a dedicated user and an empty database.

So first go to MySQL mode: `mysql -u root -p` and enter the password you have defined during the installation process.

Then execute following queries:

```SQL
CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<your password goes here>';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
FLUSH PRIVILEGES;
EXIT
```

## Installation and configuration

Get Pydio Cells binary:

```sh
cd /home/pydio
# Home edition
wget https://download.pydio.com/pub/cells/release/1.4.1/linux-amd64/cells
sudo chmod u+x cells
sudo chown pydio.pydio cells

# Enterprise edition
wget https://download.pydio.com/pub/cells-enterprise/release/1.4.2/linux-amd64/cells-enterprise
sudo chmod u+x cells-enterprise
sudo chown pydio.pydio cells-enterprise
```

If you need to use the standard http (80) or https (443) port, please execute this command:

```sh
sudo setcap 'cap_net_bind_service=+ep' cells
```

Switch to the **pydio** user to run the installation and start the app:

```sh
su - pydio
```

Execute the command below and follow the instructions.

```sh
./cells install
```

**Before you start installing here's two of the most important parameters that you need to understand:**

```text
INTERNAL_URL : address where the application http server is bound to. It MUST contain a server name and a port.
EXTERNAL_URL : url the end user will use to connect to the application.

Example:
If you want your application to run on the localhost at port 8080 and use the url mycells.mypydio.com, then set INTERNAL_URL to localhost:8080 and EXTERNAL_URL to mycells.mypydio.com
After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:
```

You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details on the installation process.
After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

```sh
./cells start
```

## Troubleshooting

### Database

If you have a problem with your database, you might already want to refer to the official documentation and insure you are correctly set up:

- [Official MySQL installation guide](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)
- [Official mariaDB installation guide](https://downloads.mariadb.org/mariadb/repositories/#mirror=cnrs&distro=Ubuntu&version=10.3)

#### Installing maria DB 10.2 fails

_During the MariaDB installation process, when trying to add the repository recommnnded by the MariaDB official documentation, you get such an error: `E: The repository 'http://mirror2.hs-esslingen.de/mariadb/repo/10.2/ubuntu bionic Release' does not have a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.`_

This happens because the tried repository does not exposes the compulsory release file. To solve this issue, go back to the [official MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=Ubuntu&version=10.2&distro_release=bionic--ubuntu_bionic) and try with another mirror.

For the record, despite the error seen above, the repository might have been added. To unregister it, do for instance:

```sh
sudo add-apt-repository --remove 'deb [arch=amd64] http://mirror2.hs-esslingen.de/mariadb/repo/10.2/ubuntu bionic main'
```