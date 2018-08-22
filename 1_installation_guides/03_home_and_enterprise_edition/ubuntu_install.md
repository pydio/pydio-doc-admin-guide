_This guide describes the steps required to have Pydio Cells running on Ubuntu._

[:image-popup:1_installation_guides/logos-os/logo-ubuntu.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Ubuntu version:

- Ubuntu 18.04 LTS (Bionic Beaver)
- Ubuntu 16.04 LTS (Xenial Xerus)

You can also find hints to prepare a 14.04 environment at the bottom of this page.

#### Dedicated User

It's highly recommend to run Pydio Cells with a dedicated user.

In this guide, we use **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
sudo passwd pydio
```

### Database

Pydio Cells can be installed with both MySQL Server (v5.6 or higher) and MariaDB (v10.2 or higher).

#### MySQL server

```sh
# On Ubuntu 18.04
sudo apt-get install mysql-server-5.7
# On Ubuntu 16.04
sudo apt-get install mysql-server-5.6
```

#### MariaDB server

You currently must use MariaDB 10.3, here is the [official installation guide on the MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=Ubuntu&version=10.3).

Simply enter your system's characteristics there and follow the detailed instructions.

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

### PHP & PHP FPM

> **WARNING**:  
> if you are installing Pydio Cells on an Ubuntu Server (rather than Desktop), you have to enable the universe repository prior to launch the php installation, you can do so by doing the following:
> ```sh
> sudo add-apt-repository universe
> ```

To install php and its packages, use following command:

```sh
sudo apt install php php-fpm php-gd php-curl php-intl php-xml
```

For PHP FPM you can choose to use TCP or Unix sockets depending on your preferences. Here are configuration steps for both solutions.

#### TCP Socket

Configure php-fpm to listen on port 9000:

```sh
# Beware to replace with your installed version of PHP in the command
echo "listen = 9000" | sudo tee -a /etc/php/<version>/fpm/php-fpm.conf
# for instance
echo "listen = 9000" | sudo tee -a /etc/php/7.2/fpm/php-fpm.conf

```

#### UNIX Socket

You have to insure the user that runs the Pydio Cells binary has sufficient rights on the socket.
You have many options, we usually add the corresponding user to the default `www-data` group and change the `listen.owner` directive of the fpm configuration file.

To do so, edit the `/etc/php/${PHP_VERSION}/fpm/pool.d/www.conf` conf file to have something like:

```sh
...content omitted...
#listen.owner= <the_correct_user>
listen.owner= pydio
listen.group= www-data
...content omitted...
```

You might also uncomment the `listen.mode = 0660` line if you want to be more restrictive.

Then, add the pydio user to www-data group and add write permission to the www-data group to the php folder:

```sh
# addgroup <the_correct_user> www-data, for instance:
sudo addgroup pydio www-data
sudo chmod g+w /run/php
```

Note: if you were logged in as user `pydio` when you did `su -`, you have to log out and back in for the permission update to be effective.

#### Finalisation

Restart and enable PHP-FPM service with these commands (after replacing the version)

```sh
sudo systemctl enable php<version>-fpm
sudo systemctl restart php<version>-fpm
# For instance
sudo systemctl enable php7.2-fpm
sudo systemctl restart php7.2-fpm
```

#### Default web serveur

Depending on your installation of Ubuntu (and also if you installed php-fpm from the universe repository on Ubuntu Server), you may have Apache2 already configured and running by default on your server. If you want to use default `80`and `443` ports for your Pydio Cells instance, you have to disable and stop the Apache 2 service:

```sh
sudo systemctl stop apache2
sudo systemctl disable apache2
```

## Installation and configuration

Get Pydio Cells binary:

```sh
# Home edition
wget https://download.pydio.com/pub/cells/release/1.0.3/linux-amd64/cells
sudo chmod u+x cells
sudo chown pydio.pydio cells
# Enterprise edition
wget https://download.pydio.com/pub/cells-enterprise/release/1.0.3/linux-amd64/cells-enterprise
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

You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details on the installation process.
After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

```sh
./cells start
```

## Troubleshooting

### Networking issues

_ You got this kind of error: `ERROR   nats    Could not run   {“error”: “No private IP address found, and explicit IP not provided”}`_

The simplest way is to create an alias on the network interface with a 10.0.X address.
For example, if the main interface is eth0, just add the following in the `/etc/network/interface` file.

```conf
auto eth0:1
allow-hotplug eth0:1
iface eth0:1 inet static
address 10.0.0.1
netmask 255.255.255.0
```

Then restart networking services.

#### Unable to bind port 443

_You have configured the bind URL with port 443 and enable redirection of port 80. You get an error page "Unable to connect" when you try to connect_.

Check the error log, if you sse such an error:

```sh
2018-05-21T10:55:57.532+0200   ERROR   pydio.grpc.gateway.proxy   Could not run   {"error": "listen tcp :443: bind: permission denied"}
```

You probably did not give permission to the `cells` binary file to use reserved ports. To fix this:

```sh
sudo setcap 'cap_net_bind_service=+ep' cells
```

### PHP-FPM & WebSockets

You can always look at the webserver's error file located in `~/.config/pydio/cells/logs/caddy_errors.log`.

- The php-fpm Service might not be started, you can look at its status using : `sudo service php<version>-fpm status` and then `sudo service php<version>-fpm start` if needed.

- Forgot to Add `listen = 9000` to the php-fpm.conf file if you're using the TCP socket.
- Forgot to change the `listen.owner` or `listen.group` directives, located in `<php-fpm path>/pool.d/www.conf` for the UNIX socket users.

### Database

If you have a problem with your database, you might already want to refer to the official documentation and insure you are correctly set up:

- [Official MySQL installation guide](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)
- [Official mariaDB installation guide](https://downloads.mariadb.org/mariadb/repositories/#mirror=cnrs&distro=Ubuntu&version=10.2)

#### Installing maria DB 10.2 fails

_During the MariaDB installation process, when trying to add the repository recommnnded by the MariaDB official documentation, you get such an error: `E: The repository 'http://mirror2.hs-esslingen.de/mariadb/repo/10.2/ubuntu bionic Release' does not have a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.`_

This happens because the tried repository does not exposes the compulsory release file. To solve this issue, go back to the [official MariaDB website](https://downloads.mariadb.org/mariadb/repositories/#distro=Ubuntu&version=10.2&distro_release=bionic--ubuntu_bionic) and try with another mirror.

For the record, despite the error seen above, the repository might have been added. To unregister it, do for instance:

```sh
sudo add-apt-repository --remove 'deb [arch=amd64] http://mirror2.hs-esslingen.de/mariadb/repo/10.2/ubuntu bionic main'
```

## Legacy Ubuntu version

### UBUNTU 14

#### Install Database

To install MySQL use : `sudo apt install mysql-server`

or for mariaDB use : `sudo apt install mariadb-server`

#### Install PHP

You can install PHP 7 on ubuntu 14 by following those steps:

- `sudo apt-get update`
- `sudo apt-get install python-software-properties`(in case you don't have it)
- `sudo add-apt-repository ppa:ondrej/php`
- `sudo apt-get update`
- you can install either php 7.0 or php 7.1 to install only php client without apache2 you have to use the following commands :
- for php 7.0
  `sudo apt-get install php7.0-cli`
  then for the other packages `sudo apt-get install php7.0-dom php7.0-curl php7.0-gd php7.0-intl`

- for php 7.1

```sh
sudo apt-get install php7.1-cli
sudo apt-get install php7.1-dom php7.1-curl php7.1-gd php7.1-intl
```

or use the default repositories and get php 5.
To install php 5 and relevant related packages, use following command:

```sh
sudo apt install -y php5-cli php5-fpm php5-gd php5-xmlrpc php5-intl php5-curl
```

Additional guides to get more information:

- [Install mysql](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-14-04)
- [Install mariaDB](https://www.vultr.com/docs/install-mariadb-on-ubuntu-14-04)
