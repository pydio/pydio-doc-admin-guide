_In this guide, we will take you through all the steps required for you to have Pydio Cells running on a CentOs/RHEL 7 server._

### Additional repos for CentOS 7.
By default, the version of some packages such as PHP or MySQL (MariaDB) is far from current released version. Therefore, we need to use some extra repositories to get recent versions.

#### EPEL release
`sudo yum install epel-release scl-utils`

##### Software collection release

CentOS: `sudo yum install centos-release-scl`

RedHat: `sudo yum-config-manager --enable rhel-server-rhscl-7-rpms`

### Database

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
CREATE USER 'pydio'@'localhost' IDENTIFIED BY 'pydio';
UPDATE mysql.user SET Password=PASSWORD('your-password-here') WHERE USER='pydio' AND Host='localhost';
# Create new database
CREATE DATABASE cells;
# Grant permission
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
# Flush permission:
FLUSH PRIVILEGES;
```

### Creation of dedicated system account for Pydio Cells

It's highly recommend to run Pydio Cells with a dedicated `pydio` user.

In this how-to, we use **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory, execute this command:

`sudo useradd -m pydio `

### SELinux

There is no available configuration of SELinux for Pydio Cells. Please make sure that SELinux is disabled or running in permissive mode.

To temporary disable SELinux: `sudo setenforce 0`.

You can also permanently disable SELinux in `/etc/selinux/config`.

## PHP-FPM

Install PHP-FPM and some mandatory extensions. In this example, we use PHP version 7.1. However, you can use 7.0 or 7.2 instead.

`sudo yum install rh-php71-php-fpm rh-php71-php-common rh-php71-php-intl rh-php71-php-gd rh-php71-php-mbstring rh-php71-php-xml rh-php71-php-curl rh-php71-php-opcache `

In next step, we change default Unix **user** for the PHP-FPM worker pool from **apache** to **pydio**.

By default, this service is listening on 9000 port, and we could change it here too if necessary.

```
sudo vi /etc/opt/rh/rh-php71/php-fpm.d/www.conf

; ... some is omitted

; Unix user/group of processes
; RPM: apache user chosen to provide access to the same directories as httpd
; user = apache
user = pydio
; RPM: Keep a group allowed to write in log dir.
group = apache

listen = 127.0.0.1:9000
```

Then enable and start the PHP-FPM service:

```bash
sudo systemctl enable rh-php71-php-fpm
sudo systemctl start rh-php71-php-fpm
```

## Installation and configuration of Pydio Cells

After having installed both php-fpm and database services, you can now install and configure Pydio Cells.

Log in or switch to **pydio** user. 

```
wget https://download.pydio.com/pub/cells/release/0.9.2/linux-amd64/cells
chmod +x cells
```

### Setup Pydio Cells

You have two ways to setup Pydio Cells after launching the first command: by command line or by web. In this how-to, we use the web interface to setup.

```
[root@localhost ~]# su - pydio
[pydio@localhost ~]$ ./cells install
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
Select url and port for **Cells** service.

> Note: Because **Cells** is launched by an non-root user, so you cannot use any ports less than 1024 in CentOS. In order to be able to open 443 or 80 port, please execute this command: `setcap 'cap_net_bind_service=+ep' /home/pydio/cells`

```
✔ Browser-based (requires a browser access)
Use the arrow keys to navigate: ↓ ↑ → ←
? Bind Url (ip:port or yourdomain.tld that the webserver will listen. If internal and external urls differ, use internal here):
+   Other
  ▸ http://192.168.0.133:8080
    http://localhost:8080
    http://0.0.0.0:8080
```

Now you can use a web browser with this address to continue with the setup. At the end, the page will automatically reload and boom ... **Cells** is working.

Next time, please use this command to start pydio:

`./pydio start `

### Data and configuration files of Pydio Cells

You will find all config files/data in directory home of **pydio** user:

**Configuration of all services of** **Cells**: /home/pydio/.config/pydio/cells/pydio.json

**Data**: /home/pydio/.config/pydio/cells/data

**PHP files for frontend**: /home/pydio/.config/pydio/cells/static/pydio

## Monitoring Pydio Cells' services by Supervisord

- Install supervisor: `yum install supervisor`
- Create a new file /etc/supervisord.d/cell.ini with following content:

```
[program:cells]
command=/home/pydio/cells start
directory=/home/pydio       ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
;priority=999                  ; the relative start priority (default 999)
autostart=true                ; start at supervisord start (default: true)
autorestart=unexpected        ; whether/when to restart (default: unexpected)
startsecs=15                   ; number of secs prog must stay running (def. 1)
startretries=5                ; max # of serial start failures (default 3)
exitcodes=0,2                 ; 'expected' exit codes for process (default 0,2)
stopsignal=INT               ; signal used to kill process (default TERM)
stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
user=pydio                 ; setuid to this UNIX account to run the program

redirect_stderr=true          ; redirect proc stderr to stdout (default false)
stdout_logfile=/home/pydio/.config/pydio/cells/logs/cells.out
stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10     ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
stderr_logfile=/home/pydio/.config/pydio/cells/logs/cell_err.log        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;environment=TMPDIR="/tmp",B="2"       ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)
```

- Enable supervisor start with system `systemctl enable supervisor && systemctl start supervisor`
- Update new program to supervisor: `supervisorctl update`
- Start cell program in supervisor: `supervisorctl start cells `

You can test this config by restarting the machine and **Pydio Cells** now is launched by supervisord. To watch the output, try this command:
`tail -f /home/pydio/.config/pydio/cells/logs/cells.out`

## Configure cells with systemd service

Create new file /etc/systemd/system/cells.service with content:

```
[Unit]
Description=Pydio Cells
Documentation=https://pydio.com
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/home/pydio/cells
[Service]
WorkingDirectory=/home/pydio/.config/
User=pydio
Group=pydio
PermissionsStartOnly=true
#ExecStartPre=echo \"Start pydio cells-enterprise service\""
ExecStart=/home/pydio/cells start
Restart=on-failure
StandardOutput=journal
StandardError=inherit
LimitNOFILE=65536
TimeoutStopSec=5
KillSignal=INT
SendSIGKILL=yes
SuccessExitStatus=0
[Install]
WantedBy=multi-user.target

```

Then enable cells service:
`systemctl enable cells `
`systemctl start cells `

The output of cells service can be seen via journal service

`journalctl -f `

## Troubleshooting

### Frontend

#### SELinux is enforced
If, after a successful installation and when you try to navigate to the main application page with your browser, you land on a blank page with following message:

> Access denied.

Insure you have modified SELinux to be in permissive mode.  

