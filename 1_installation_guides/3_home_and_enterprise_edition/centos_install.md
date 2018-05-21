_This guide describes the steps required to have Pydio Cells running on a CentOs/RHEL 7 server._

[:image-popup:1_installation_guides/logos-os/logo-centos.png]

## Prerequisites 

### Repositories
The version of packages such as PHP or MySQL (MariaDB) is outdated by default. We need to use extra repositories with more recent versions.

#### EPEL release
```bash
sudo yum install epel-release scl-utils
```

#### Software collection release
CentOS: 
```bash
sudo yum install centos-release-scl
```

RedHat:
```bash
sudo yum-config-manager --enable rhel-server-rhscl-7-rpms
```

### Database

You can either use MySQL or MariaDB.

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

#### MariaDB
To install MariaDB, follow below steps

```bash
# install the RPM
sudo yum install rh-mariadb102-mariadb-server
# start MariaDB after reboot
sudo systemctl enable rh-mariadb102-mariadb
# start the service
sudo systemctl start rh-mariadb102-mariadb
```

#### Post install configuration
By default, a new database will be created by the system during the installation process. You only need a user with database management permissions.

If you would rather do it manually, you may create a dedicated user and an empty database by executing the following SQL queries :

```SQL
CREATE USER 'cells'@'localhost' IDENTIFIED BY '<your-password-here>';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'cells'@'localhost';
FLUSH PRIVILEGES;
```

### SELinux
There is no available configuration of SELinux for Pydio Cells. Please make sure that SELinux is disabled or running in permissive mode.

To temporary disable SELinux: `sudo setenforce 0`.

You can also permanently disable SELinux in `/etc/selinux/config`.

### PHP-FPM
In this example we use PHP version 7.1, but you can use any version >= 5.5.9.

```bash
sudo yum install rh-php71-php-fpm rh-php71-php-common rh-php71-php-intl rh-php71-php-gd rh-php71-php-mbstring rh-php71-php-xml rh-php71-php-curl rh-php71-php-opcache
```

The default **user** for the PHP-FPM worker pool needs to be changed to **cells**. Change the port the service is listening to if required.

```bash
sudo vi /etc/opt/rh/rh-php71/php-fpm.d/www.conf

; ... omitted

; Unix user/group of processes
; RPM: apache user chosen to provide access to the same directories as httpd
; user = apache
user = cells
; RPM: Keep a group allowed to write in log dir.
group = apache

listen = 127.0.0.1:9000
```

Then enable and start the PHP-FPM service:

```bash
sudo systemctl enable rh-php71-php-fpm
sudo systemctl start rh-php71-php-fpm
```

### Dedicated User
It is recommended to use a dedicated user to run Pydio Cells.

In this guide, we use **cells** and its home directory **/home/cells**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m cells
sudo passwd cells
```

Switch to this user to run the installation

```sh
su - cells
```

## Install Pydio Cells

```sh
wget https://download.pydio.com/pub/cells/release/1.0.0/linux-amd64/cells
chmod u+x cells
# if you need to use the standard http (80) or https (443) port, please execute this command:
setcap 'cap_net_bind_service=+ep' cells
./cells install
```

Follow the short set of instructions to finish off the Pydio Cells installation. You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details on the installation process.

## Post-installation

### Manual start

```bash
./cells start
```

### Files location

**Configuration**: /home/cells/.config/pydio/cells/pydio.json

**Data**: /home/cells/.config/pydio/cells/data

**PHP files for frontend**: /home/cells/.config/pydio/cells/static/pydio

### Monitoring

#### Supervisord

##### Install
```bash
yum install supervisor
systemctl enable supervisor && systemctl start supervisor
```

##### Configure
Create and edit a file _/etc/supervisord.d/cell.ini_ with the following content:

```
[program:cells]
command=/home/cells/cells start
directory=/home/cells         ; directory to cwd to before exec (def no cwd)
;umask=022                    ; umask for process (default None)
;priority=999                 ; the relative start priority (default 999)
autostart=true                ; start at supervisord start (default: true)
autorestart=unexpected        ; whether/when to restart (default: unexpected)
startsecs=15                  ; number of secs prog must stay running (def. 1)
startretries=5                ; max # of serial start failures (default 3)
exitcodes=0,2                 ; 'expected' exit codes for process (default 0,2)
stopsignal=INT                ; signal used to kill process (default TERM)
stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false            ; SIGKILL the UNIX process group (def false)
user=cells                    ; setuid to this UNIX account to run the program

redirect_stderr=true          ; redirect proc stderr to stdout (default false)
stdout_logfile=/home/cells/.config/pydio/cells/logs/cells.out
stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10     ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false  ; emit events on stdout writes (default false)
stderr_logfile=/home/cells/.config/pydio/cells/logs/cell_err.log        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;environment=TMPDIR="/tmp",B="2"       ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)
```

Update supervisor and start cells

```bash
supervisorctl update
supervisorctl start cells
```

#### Systemd

##### Configure
Create and edit a file _/etc/systemd/system/cells.service_ with the following content:

```
[Unit]
Description=Pydio Cells
Documentation=https://pydio.com
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/home/cells/cells
[Service]
WorkingDirectory=/home/cells/.config/
User=cells
Group=cells
PermissionsStartOnly=true
#ExecStartPre=echo \"Start pydio cells-enterprise service\""
ExecStart=/home/cells/cells start
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

Enable and start cells
`systemctl enable cells `
`systemctl start cells `

##### Logs

```bash
journalctl -f cells
```

## Troubleshooting

### Frontend

#### SELinux is enforced

If, after a successful installation and when you try to navigate to the main application page with your browser, you land on a blank page with following message:

> Access denied.

Insure you have modified SELinux to be in permissive mode.
