
_This guide describes the steps required to have Pydio Cells running on Debian and Ubuntu_.

[:image:2_running_cells_in_production/logos-os/logo-debian.png]

[:image:2_running_cells_in_production/logos-os/logo-ubuntu.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Debian or Ubuntu versions:

- Debian 10 (Buster) LTS
- Debian 9 (Stretch) LTS / Raspbian Stretch
- Debian 8 (Jessie) LTS / Raspbian Jessie
- Ubuntu 20.04 (Focal Fossa)
- Ubuntu 18.04 (Bionic Beaver)
- Ubuntu 16.04 (Xenial Xerus)

#### Dedicated User

It is highly recommended to run Pydio Cells with a dedicated user.

In this guide, we use a dedicated user **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
```

#### Installation location

By default, Cells is installed in a subdirectory in the `home` folder of the user that performs the installation. For instance, using the `pydio` user, it is `/home/pydio/.config/pydio/cells`.

This is not recommanded for production use: we rather recommand installing Cells in `/var/cells`.

To do so, you should follow the below steps:

- Define a `CELLS_WORKING_DIR` environment variable
- Put your binary in a dedicated location, we recommand `/opt/pydio/bin/`
- Create a symbolic link from a location that is in your `PATH` that points toward the downloaded binary

Typically:

```sh
# as root user

# Create correct locations
mkdir -p /opt/pydio/bin /var/cells

# Define a system wide environment variable
tee -a /etc/profile.d/cells-env.sh << EOF
#Defines the path to Cells' working dir.
export CELLS_WORKING_DIR=/var/cells
EOF

# Retrieve the binary
downloadUrl=https://download.pydio.com/latest/cells/release/{latest}/linux-amd64/cells
# Or for the Enterprise Distribution
downloadUrl=https://download.pydio.com/latest/cells-enterprise/release/{latest}/linux-amd64/cells-enterprise
wget --output-document=/opt/pydio/bin/cells $downloadUrl

# Put a sym link in the path
ln -s /opt/pydio/bin/cells /usr/local/bin/cells

# Insure permissions are correctly set
chown -R pydio:pydio /var/cells /opt/pydio
chmod 0755 /etc/profile.d/cells-env.sh
chmod 0755 /opt/pydio/bin/cells
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

If you have followed the instruction from the **Installation location** section above, you should already have the Cells binary in your path, to double check, simply run: `cells version`, otherwise, download the binary and make it executable:

```sh
# Use this url as is, you will be redirected to latest version automatically
wget https://download.pydio.com/latest/cells/release/{latest}/linux-amd64/cells
sudo chmod u+x cells
```

If you need to use the standard http (80) or https (443) port, please execute this command:

```sh
setcap 'cap_net_bind_service=+ep' <path-to-your-binary>
```

Switch to the **pydio** user to run the installation and start the app:

```sh
su - pydio
```

Execute the command below and follow the instructions.

```
cells install
```

```sh
cells start
```

You can now access your Cells instance on port 8080 with `HTTPS` (self-signed), for instance `https://localhost:8080` or `https://<server ip or domain>:8080`.

To select a different interface and port for cells, use the following command answer yes and start to configure.

```
cells config sites
```

**It is advised to add Cells as a service with systemd or supervisor - See our Knowledge Base for the dedicated Guides.**

## Troubleshooting

Generally, you might want to have a look at the log file that is located in `$CELLS_WORKING_DIR/logs`.

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
