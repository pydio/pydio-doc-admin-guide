
_This guide describes the steps required to have Pydio Cells running on Debian 8 and Debian 9._

[:image-popup:1_installation_guides/logos-os/logo-debian.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Debian or Raspbian versions:

- Stretch 9 (stable) / Raspbian Stretch
- Jessie 8 (LTS) / Raspbian Jessie

### Database

Pydio Cells can be installed with both MySQL Server (v5.6 or higher) and MariaDB.

#### MySQL

To install the appropriate version, you first have to configure the mysql-server installer:

```bash
wget https://repo.mysql.com/mysql-apt-config_0.8.9-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.9-1_all.deb
```

To configure the version of MySQL Server you will use:

- Select the `MySQL Server & Cluster` option and press `Enter`
- Select the correct option (`mysql-5.6` or `mysql-5.7`) and press `Enter`
- Back on the first list, select `Ok`  and press `Enter`

You can then install the server:

```bash
sudo apt update
sudo apt install mysql-server
```

#### MariaDB

* Add the repository  
    You first need to add the MariadDB repository key and add the package repository

    ##### Debian 8

    ``` bash
    sudo apt-get install software-properties-common
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xcbcb082a1bb943db
    sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://www.ftp.saix.net/DB/mariadb/repo/10.1/debian jessie main'
    ```

    ##### Debian 9

    ``` bash
    sudo apt-get install software-properties-common
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
    sudo add-apt-repository 'deb [arch=amd64] http://www.ftp.saix.net/DB/mariadb/repo/10.1/debian stretch main'
    ```

* Install

    You can now install MariaDB server with:

    ``` bash
    sudo apt update
    sudo apt install mariadb-server
    ```

### PHP

- **[DEBIAN 8 ONLY]  Add the PHP 7 repository**
    PHP 7 package are not available from official package list, so you need to add them.

```sh
sudo echo "deb http://packages.dotdeb.org jessie all" > /etc/apt/sources.list.d/dotdeb.list
wget -O- https://www.dotdeb.org/dotdeb.gpg | sudo apt-key add
sudo apt update
```

- **Install**

```sh
sudo apt install php7.0 php7.0-fpm php7.0-gd php7.0-curl php7.0-intl php7.0-xml
```

- **FPM configuration**

    Pydio can communicate with fpm though TCP socket or UNIX socket. You have to edit the fpm config file and tell whether to listen on TCP port 9000 or to listen on UNIX socket at /var/run/php7-fpm.sock. Here are steps to do so.

    #### TCP Socket

    You simply have to add this directive ***listen = 9000*** to the fpm configs file

    ```sh
    # Note that you must execute the *2* following lines at a time.
    sudo sed -i '$ a\
    listen=9000' /etc/php/7.0/fpm/php-fpm.conf
    ```

    #### UNIX Socket

    You have to insure the user that runs the Pydio Cells binary has sufficient rights on the socket.
    You have many options, we usually add the corresponding user to the default `www-data` group and change the `listen.owner` directive of the fpm configuration file by doing:
    
    ```sh
    # as root user
    # addgroup <the_correct_user> www-data, for instance:
    addgroup cells www-data
    ```
    And then open the `/etc/php/${PHP_VERSION}/fpm/pool.d/www.conf` conf file to have somthing like:

    ```sh
    #listen.owner= <the_correct_user> 
    listen.owner= cells
    listen.group= www-data
    ```

- **Finalisation**

    Enable and restart PHP FPM service to apply the changes:

    ```sh
    sudo systemctl enable php7.0-fpm
    sudo systemctl restart php7.0-fpm
    ```

#### Database Configuration

By default, a new database will be created by the system during the installation process. You only need a user with database management permissions.

If you would rather do it manually, you may create a dedicated user and an empty database:

```sh
# Get into the mysql mode
mysql -u root -p
```

and execute following queries:

```SQL
CREATE USER 'cells'@'localhost' IDENTIFIED BY '<change password here>';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'cells'@'localhost';
FLUSH PRIVILEGES;
```

## Pydio Cells installation

#### Downloading the binary file

You will need the Pydio Cells binary that is ~165MB. You might want to already start dowloading it while going through the _Requirements_ part of this guide.

```sh
wget https://download.pydio.com/pub/cells/release/0.9.0/linux-amd64/cells
```

### Rights

First, give execution rights to the binary:

```sh
sudo chmod u+x cells
```

### On HTTP standard ports: 80 and 443

By default you cannot use those ports if you are not a root user.  
To be able to bind those ports to Pydio, you need to give the binary the rights to use them even though it's not launched as a root user.

You can use this command:

```sh
sudo setcap CAP_NET_BIND_SERVICE=+eip cells
# Replace <cells> with full path to the Pydio Cells binary.
```

### Install

Execute the command below and follow the instructions.

```sh
./cells install
```

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

```sh
./cells start
```

## Troubleshooting

Generally, you might want to have a look at the log file that is located in `~/.config/pydio/cells/logs`.

### FPM and Websocket issues

_You have installed and started Pydio Cells with no problem, but when you connect, you only see a blank page with `File not found.` message._

It's usually related to a misconfiguration of the websocket, you should check the following

#### Is the php-fpm service running?

To check service status, do:

```sh
sudo systemctl status php<version>-fpm
# Enable and start it, if necessary
sudo systemctl enable php<version>-fpm
sudo systemctl start php<version>-fpm
```

#### Is the socket correctly configured?

If you are using the TCP socket, you might have forgot to add `listen = 9000` to the php-fpm.conf file.

For the UNIX socket users, you might have forgot to change the `listen.owner` or `listen.group` directives located in the `<php-fpm path>/pool.d/www.conf` file.

### Database server issue

_After start, the web page is unreachable and you see a bunch of errors starting with: `ERROR	pydio.grpc.meta	Failed to init DB provider	{"error": "Error 1071: Specified key was too long; max key length is 767 bytes handling data_meta_0.1.sql"}`._

You might have an unsupported version of the mysql server: you should use MySQL server version 5.6 or higher or MariaDB version 10.2 or higher. 

### Various

_You see this error: `/lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.14' not found`_

The version of libc6 is outdated. Run these commands to upgrade it.

```sh
sudo apt-get update
sudo apt-get install libc6
```
