
_In this guide, we will take you through all the steps required for you to have Pydio Cells running on a CentOs/RHEL 7 server._

### Additional repos for CentOS 7.
By default, the version of some packages such as PHP or MySQL (MariaDB) is far from current released version. Therefore, we need to use some extra repositories to get recent versions.

#### EPEL release
`sudo yum install epel-release scl-utils`

##### Software collection release

CentOS: `sudo yum install centos-release-scl`

RedHat: `sudo yum-config-manager --enable rhel-server-rhscl-7-rpms`

### 1 Database
*You can skip this step if you already have a database*

Currently, you can use either MySQL or MariaDB as backend for Pydio Cells.

##### MySQL


To install MySQL, first install MySQL 5.6 official community release repository.

`sudo rpm -i  http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm`

> Note: don't forget to execute `sudo yum update`

You must then
```bash
# install mysql-community-server package
sudo yum install mysql-community-server

# Set mysqld to start after reboot
sudo systemctl enable mysqld
# start the service now
sudo systemctl start mysqld
```

##### MariaDB

To install MariaDB, follow below steps

```bash
# install the RPM
sudo yum install rh-mariadb102-mariadb-server
# start MariaDB after reboot
sudo systemctl enable rh-mariadb102-mariadb
# start the service
sudo systemctl start rh-mariadb102-mariadb
```

##### Post install configuration
It's now time to create a user and a database for Cells.

```
# Go to mysql mode
sudo mysql -u root
# Create new user and set password
CREATE USER 'cell'@'localhost' IDENTIFIED BY 'cell';
UPDATE mysql.user SET Password=PASSWORD('your-password-here') WHERE USER='cell' AND Host='localhost';
# Create new database
CREATE DATABASE pydio;
# Grant permission
GRANT ALL PRIVILEGES ON pydio.* to 'cell'@'localhost';
# Flush permission:
FLUSH PRIVILEGES;
```

### 2 Creation of dedicated system account for Pydio Cells

It's highly recommend to run Pydio Cells with a dedicated `cell` user.

In this how-to, we use **cell** and its home directory **/home/cell**.

In order to create a new user and its home directory, execute this command:

`sudo useradd -m cell `

### 3 SELinux

There is no available configuration of SELinux for Pydio Cells. Please make sure that SELinux is disabled or running in permissive mode.

To temporary disable SELinux: `sudo setenforce 0`.

You can also permanently disable SELinux in `/etc/selinux/config`.

## 4 PHP-FPM

Install PHP-FPM and some mandatory extensions. In this example, we use PHP version 7.1. However, you can use 7.0 or 7.2 instead.

`sudo yum install rh-php71-php-fpm rh-php71-php-common rh-php71-php-intl rh-php71-php-gd rh-php71-php-mbstring rh-php71-php-xml rh-php71-php-curl rh-php71-php-opcache `

In next step, we change default Unix **user** for the PHP-FPM worker pool from **apache** to **cell**.

By default, this service is listening on 9000 port, and we could change it here too if necessary.

```
sudo vi /etc/opt/rh/rh-php71/php-fpm.d/www.conf

; ... some is omitted

; Unix user/group of processes
; RPM: apache user chosen to provide access to the same directories as httpd
; user = apache
user = cell
; RPM: Keep a group allowed to write in log dir.
group = apache

listen = 127.0.0.1:9000
```

Then enable and start the PHP-FPM service:

```bash
sudo systemctl enable rh-php71-php-fpm
sudo systemctl start rh-php71-php-fpm
```

## 5 Installation and configuration of Pydio Cells

After having installed both php-fpm and database services, you can now install and configure Pydio Cells.

Log in or switch to **cell** user.

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

### 5.1 Setup Pydio Cells

You have two ways to setup Pydio Cells after launching the first command: by command line or by web. In this how-to, we use the web interface to setup.

```
su - cell
./pydio install
Welcome to Pydio Cells installation
Pydio Cells services will be configured to run on this machine. Make sure to prepare the following data
 - IPs and ports for binding the webserver to outside world
 - MySQL 5.6+ (or MariaDB equivalent) server access
 - PHP-FPM 7+ for running frontend
Pick your installation mode when you are ready.

Use the arrow keys to navigate: ↓ ↑ → ←
? Installation mode:
  ▸ Browser-based (requires a browser access)
    Command line (performed in this terminal)
```
Select url and port for **pydio cell** service.

> Note: Because pydio cell is launched by an non-root user, so you cannot use any ports less than 1024 in CentOS.

```
✔ Browser-based (requires a browser access)
Use the arrow keys to navigate: ↓ ↑ → ←
? Bind Url (ip:port or yourdomain.tld that the webserver will listen. If internal and external urls differ, use internal here):
+   Other
  ▸ http://192.168.0.133:8080
    http://localhost:8080
    http://0.0.0.0:8080
```

Now you can use a web browser with this address to continue with the setup. At the end, the page will automatically reload and boom ... **Pydio Cells** is working.

Next time, please use this command to start pydio:

`./pydio start `

### Data and configuration files of Pydio Cells

You will find all config files/data in directory home of **cell** user:

**Config of all services of** **Pydio Cells**: /home/cell/.config/pydio/cells/pydio.json

**Data**: /home/cell/.config/pydio/cells/data

**PHP files for frontend**: /home/cell/.config/pydio/cells/static/pydio

## Troubleshooting

### Frontend

#### SELinux is enforced
If, after a successful installation and when you try to navigate to the main application page with your browser, you land on a blank page with following message:

> Access denied.

Insure you have modified SELinux to be in permissive mode.  
