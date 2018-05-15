_In this guide, you will find all the steps required for installation and configure Cells Enterprise with pre-built vm image._

The image of ovf can be download from https://download.pydio.com/pub/cells-enterprise/release/VERSION/ovf/Cells-Enterprise-OVF-VERSION.zip

where "VERSION" is release version of cells.

For example: https://download.pydio.com/pub/cells-enterprise/release/0.9.2/ovf/Cells-Enterprise-OVF-0.9.2.zip

A md5 file is also available on the same location for integrity verification.

You will find out some text file accompanied with zip file:

**EULA**: End-user license aggrement
**Import_to_Hyper_V.txt**: instructions for importing vm image into Hyper-V system
**Import_to_VMWare_System.txt**: some notes for importing vm image into different version of VMWare system

In this guide, virtualbox is use for importing ovf file.

Virtual Machine requirements

- RAM : 4G
- CPU : 2 vCores
- CPU support: VT-x/AMD-V
- NICs: bridged adaptor

## 2 Predefined accounts

There are two pre-build accounts accessible via ssh
username: *root*
password: *PydioVAPP*

username: *pydio*
password: *PydioVAPP*

There are two account for mysql
username: *root*
password:

username: *pydio@localhost*
password:

A predefined database is *cells*


## 3 Setup and config at the first time
After importing vm image into VirtualBox, a specific script will be launch at the fist boot of the machine and you can interact with cells service via web browser to setup/config **cells**. Following the message in vm screen, you can see the url of service in format: https://ipaddress/

> Note: A self-singed certificate is used so please ignore the warning message on your web browser.

You just follow steps in web interface to finalize the configuration. All parameters is set by default except admin's password.

You can see on web page the progress of installation. It takes several minute to reach the end.

> Note: You are required to refresh the page manually because the self-signed certificate require a confirmation.

At this step, you can login to Cells-Enterprise with credential you've enter during the setup. It's highly recommended to restart virtual machine.

## 4 Notes for configuration

#### 4.1 systemd service
**Cell-Enterprise** service is running under **pydio** user. You find out all files in /home/pydio/.config/pydio/cells and it's configured to start at the boot time by using systemd

Manually start/stop **Cell-Enterprise**

`systemctl start cells`
`systemctl stop cells`

You can consult the output of **Cell-Enterprise** service by using command:

`journalctl -f `

start/stop some others service:

MariaDB: 
`systemctl start rh-mariadb102-mariadb `
`systemctl stop rh-mariadb102-mariadb `

PHP-FPM:
`systemctl start rh-php71-php-fpm`
`systemctl stop rh-php71-php-fpm`

#### 4.2 firewalld service

Firewalld service is active and open two ports:
443: **Cells-Enterprise**
22: **ssh**

#### 4.3 SELinux

SELinux is running in *permissive* mode
