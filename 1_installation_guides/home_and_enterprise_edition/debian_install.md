_This document will guide you through steps to have Pydio Cells configured and running on debian 8 and 9._

#### Pydio Cells

**Home Edition**
```
wget https://download.pydio.com/pub/cells/release/0.9.0/linux-amd64/cells
chmod +x cells
```
**Enterprise Edition**

```
wget https://download.pydio.com/pub/cells-enterprise/release/0.9.1/linux-amd64/cells-enterprise
chmod +x cells-enterprise
```

## Requirements installation

#### PHP


*   Add the PHP 7 repository (FOR DEBIAN 8 ONLY)

    PHP 7 package are not available from official package list, so you need to add them

``` bash
sudo echo "deb http://packages.dotdeb.org jessie all" > /etc/apt/sources.list.d/dotdeb.list
wget -O- https://www.dotdeb.org/dotdeb.gpg | sudo apt-key add
sudo apt update
```

* Install

``` bash
sudo apt install php7.0 php7.0-fpm php7.0-gd php7.0-curl php7.0-intl php7.0-xml
```

* FPM configuration

    Pydio can communicate with fpm though TCP socket or UNIX socket. You have to edit the fpm config file and tell whether to listen on TCP port 9000 or to listen on UNIX socket at /var/run/php7-fpm.sock. Here are steps to do so.

    ##### TCP Socket

    You simply have to add this directive ***listen = 9000*** to the fpm configs file

    ``` bash
    # Note that you must execute the *2* following lines at a time.
    sudo sed -i '$ a\
    listen=9000' /etc/php/7.0/fpm/php-fpm.conf
    ```

    ##### UNIX Socket

    with your favorite editor open the /var/run/php7-fpm.sock and edit it to have something similar:

    ``` bash
    listen.owner= <user>
    listen.group= <group>
    ```

    Replace `<user>` with the name of the user that run Pydio Cells and `<group>` with the name of the group the user belong to.


    Now restart/reload php-fpm to apply the changes:

    ``` bash
    sudo /etc/init.d/php7.0-fpm restart
    ```

#### MySQL
*You can skip this step if you already have a database*

You first have to configure the mysql-server installer in order to install the appropriate version
``` bash
wget https://repo.mysql.com//mysql-apt-config_0.8.9-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.9-1_all.deb
```

Press 'Enter' on the version of mysql-server you would like to install and press 'Enter' on 'OK'.

``` bash
sudo apt intall mysql-server
```

#### MariaDB install

*   Add the repository

    You first need to add the MariadDB repository key and add the package repository

    ##### Debian 8

    ``` bash
    sudo apt-get install software-properties-common
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xcbcb082a1bb943db
    sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://www.ftp.saix.net/DB/mariadb/repo/10.1/debian  jessie main'
    ```

    ##### Debian 9

    ``` bash
    sudo apt-get install software-properties-common
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
    sudo add-apt-repository 'deb [arch=amd64] http://www.ftp.saix.net/DB/mariadb/repo/10.1/debian stretch main'
    ```

*   Install

    You can now install MariaDB server with:

    ``` bash
    sudo apt update
    sudo apt install mariadb-server
    ```

#### Database  Configuration

We recommend you create a database dedicated to Pydio Cells. Here are how you can create a database named `pydio` and a user named `cellsuser` with all priviledges on it.

``` bash
# Get into the mysql mode
sudo mysql -u root -p
```

and execute the following queries:
``` SQL
/*Create new user and set password*/
CREATE USER 'cellsuser'@'localhost' IDENTIFIED BY 'password';

/* Create new database */
CREATE DATABASE pydio;

/* Grant permission */
GRANT ALL PRIVILEGES ON pydio.* to 'cellsuser'@'localhost';

/* apply changes: */
FLUSH PRIVILEGES;
```

## Pydio Cells installation

We assume you downloaded the Pydio Cells binary and saved it as `cells`.

#### Rights

First, give execution rights to the binary:

``` bash
sudo chmod u+x pydio
```

#### On HTTP standard ports: 80 and 443

By default you cannot use those ports if you are not a root user (sudo, root, etc...)
to be able to bind those ports to Pydio you need to give the binary the rights to use them even though it's not launched as a root user.

Basically to do that you can use this command :
``` bash
sudo setcap CAP_NET_BIND_SERVICE=+eip pydio
# Replace <pydio> with full path to the Pydio Cells binary.
```

#### Install

Execute the command below and follow the instructions.
``` bash
./cells install
```
**For the enterprise edition refer to this guide to get your license key allowing you to complete the installation**

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

``` bash
./cells start
```

## Troubleshooting

* The php-fpm Service might not be started, you can look at its status using : `sudo service php<version>-fpm status` and then `start` it if needed.

* Forgot to Add `listen = 9000` to the php-fpm.conf file if you're using the TCP socket.

* Forgot to change the `listen.owner=` or `listen.group=` located in ``<php-fpm path>/pool.d/www.conf`` for the UNIX socket users.

* You can look at the webserver's error file located in `~/.config/pydio/cells/logs`.

* _/lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.14' not found_ :
The version of libc6 is outdated. Run these commands to upgrade it.

```
sudo apt-get update
sudo apt-get install libc6
```
