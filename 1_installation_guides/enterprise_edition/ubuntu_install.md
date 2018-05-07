# Installation guide for Ubuntu

_In this guide, you will find all the steps required for you to have Pydio Cells running on an Ubuntu system. We will be covering the following versions: Ubuntu 14.04, Ubuntu 16.04 & Ubuntu 17.10._

#### Pydio Cells

Download the Cells Binary on your server using the following command :
```
wget https://download.pydio.com/pub/cells/release/0.9.0/linux-amd64/cells
chmod +x cells
```

## Installation

#### UBUNTU 16 & 17 :
On both of those versions as they're not old, the installation process should be the easiest.

##### Install PHP
To install php and it's packages use the following commands:
`sudo apt install php php-fpm php-gd php-curl php-intl php-xml`.

##### Install Database
*You can skip this step if you already have a database*

To install MySQL use : `sudo apt install mysql-server`

or for mariaDB use : `sudo apt install mariadb-server`

*if you missed a step during the installation process you can use `mysql_secure_installation` command to redo it again, it works for both*

#### UBUNTU 14 :

##### Install PHP
You can install PHP 7 on ubuntu 14 by following those steps:

* `sudo apt-get update`
* `sudo apt-get install python-software-properties`(in case you don't have it)
* `sudo add-apt-repository ppa:ondrej/php`
* `sudo apt-get update`
* you can install either php 7.0 or php 7.1 to install only php client without apache2 you have to use the following commands :

* for php 7.0
`sudo apt-get install php7.0-cli`
then for the other packages `sudo apt-get install php7.0-dom php7.0-curl php7.0-gd php7.0-intl`

* for php 7.1
`sudo apt-get install php7.1-cli`
:`sudo apt-get install php7.1-dom php7.1-curl php7.1-gd php7.1-intl`

or use the default repositories and get php 5.
To install php 5 and relevant related packages, use following command:
`sudo apt install -y php5-cli php5-fpm php5-gd php5-xmlrpc php5-intl php5-curl`.

##### Install Database
To install MySQL use: `sudo apt-get install mysql-server-5.6`

or mariaDB: `sudo apt-get install mariadb-server`

*If you missed a step during the installation process, you can use `mysql_secure_installation` command to redo it again. It works for both MySQL and MariaDB*


Additional guides for more informations :
* [official MySQL installation guide](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/)

* [install mysql](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-14-04)

* [official mariaDB installation guide](https://downloads.mariadb.org/mariadb/repositories/#mirror=cnrs&distro=Ubuntu&distro_release=trusty--ubuntu_trusty&version=10.2)

* [install mariaDB](https://www.vultr.com/docs/install-mariadb-on-ubuntu-14-04)


## Final steps

#### Php FPM
For php FPM you can choose to use the TCP socket or the UNIX socket we will provide you both solutions.

##### TCP Socket

You need to make sure that php-fpm is listening on port 9000. To do so,
edit it with your favorite text-editor, for example `nano /etc/php/<version>/fpm/php-fpm.conf` and add this at the end of the file `listen = 9000`.

Now restart/reload php-fpm `service php<version>-fpm reload`.

##### UNIX Socket

First step is to modify in this file``<path to php fpm>/pool.d/www.conf``the values `listen.owner= <user>` and `listen.group= <group>`(usually the group and owner have the same values, for example : you have a user named Pydio then the group will be also named Pydio) and add the user/group that will be launching Pydio.
You can also uncomment `listen.mode = 0660` if you want to restrict some parts.


Make sure to restart/reload php-fpm `service php<version>-fpm reload`.

#### Port 80 & 443
By default you cannot use those ports if you are not a root user (sudo, root, etc...)
to be able to bind those ports to Pydio you need to give the binary the rights to use them even though it's not launched as a root user.

Basically to do that you can use this command : `sudo setcap CAP_NET_BIND_SERVICE=+eip <binary>` then the binary will be able to bind the ports without root privileges.

#### Database configuration

In this section, we assume you have installed MySql server 5.7+. Adapt the following steps to your current installation
```
# Go to mysql mode
sudo mysql -u root
# Create new user and set password
CREATE USER 'cell'@'localhost' IDENTIFIED BY 'cell';
SET password for 'cell'@'localhost' = PASSWORD('your password goes here');
# Create new database
CREATE DATABASE pydio;
# Grant permission
GRANT ALL PRIVILEGES ON pydio.* to 'cell'@'localhost';
# Flush permission:
FLUSH PRIVILEGES;
```


## Starting with Pydio

First, give execution rights to the binary. For instance, you can use `sudo chmod u+x <binary>`.

Then, to launch the installer, type: `./<binary> install`. For instance: `./cells install`.

A menu will appear.

* **Installation mode** : you can use a browser or cli to install Pydio Cells.

* **bind url** : internal url for PYDIO in most common cases it's `<ip>:<port>`(example : `192.168.0.192:80`) if you are going to use SSL you should put your secure port `443` or else.

* **external url** : used to access Pydio from outside, usually it will auto fill using the field above and should stay the same.

* **Choose SSL Activation mode** : You can now choose how you're going to activate your SSL, you have 2 choices.
  * **Provide paths to certificate/key files** : if you already have a certificate/key you can use them.
  * **Generate a self-signed certificate** : we will create a self signed certificate for you, be advised this should only be used for staging purposes.


*Subsequent steps are then pretty much the same in the browser or in the CLI*

1. **Database connection** : put your database informations.

2. **PHP-FPM Detection** : the installer detects your version of php-fpm and if there are missing packages.
If you have a warning because you're using php 5.5.9, don't worry and press next.

3. **Admin User** : Pydio's Admin user informations.

4. **Advanced Settings** : you can skip this part for the time being.

5. **Apply Installation** : you will have progress bar. If you are using the web-based installer, the page will then reload automatically, so don't quit this page or press anything.

To stop Pydio you can press `ctrl + c` and then to start it again use this command
`./<binary> start`, for instance `./cells start`.



## Troubleshooting

* The php-fpm Service might not be started, you can look at its status using : `sudo service php<version>-fpm status` and then `sudo service php<version>-fpm start` it if needed.

* Forgot to Add `listen = 9000` to the php-fpm.conf file if you're using the TCP socket.

* Forgot to change the `listen.owner=` or `listen.group=` located in ``<php-fpm path>/pool.d/www.conf`` for the UNIX socket users.

* You can look at the webserver's error file located in `~/.config/pydio/cells/logs/caddy_errors.log`.
