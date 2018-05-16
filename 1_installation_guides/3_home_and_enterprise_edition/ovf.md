_In this guide, you will find all the steps required for installation and configure Cells Enterprise with pre-built vm image._

## Download OVF Package

The image of OVF can be downloaded from 

```
https://download.pydio.com/pub/cells-enterprise/release/VERSION/ovf/Cells-Enterprise-OVF-VERSION.zip
```
where "VERSION" is release version of cells. Get for example: [https://download.pydio.com/pub/cells-enterprise/release/1.0.0/ovf/Cells-Enterprise-OVF-1.0.0.zip](https://download.pydio.com/pub/cells-enterprise/release/1.0.0/ovf/Cells-Enterprise-OVF-1.0.0.zip). An md5 file is also available on the same location for integrity verification.

This Zip archive contains the OVF itself plus the following files: 

- **EULA**: End-user license aggrement
- **Import_to_Hyper_V.txt**: instructions for importing vm image into Hyper-V system
- **Import_to_VMWare_System.txt**: some notes for importing vm image into different version of VMWare system

## Launching a VM

In this guide, we will use Oracle VirtualBox to launch the virtual machine.

Virtual Machine requirements

- RAM : 4G
- CPU : 2 vCores
- CPU support: VT-x/AMD-V
- NICs: bridged adaptor

### Setup and config at the first time

After importing vm image into VirtualBox, a specific script will be launch at the fist boot of the machine and you can interact with cells service via web browser to setup/config **cells**. Following the message in vm screen, you can see the url of service in format: https://ipaddress/

> Note: A self-signed certificate is used, ignore the warning message on your web browser.

You just follow steps in web interface to finalize the configuration. All parameters is set by default except admin's password.

You can see on web page the progress of installation. It takes several minute to reach the end.

> Note: You are required to refresh the page manually because the self-signed certificate require a confirmation.

At this step, you can login to Cells-Enterprise with credential you've enter during the setup. It's highly recommended to restart virtual machine.

### Predefined accounts

If you ever need to login to the VM system, Ssh accounts and technical accounts are created as follow:

* Administrative user: *root* / *PydioVAPP* (ssh access)
* User: *pydio* / *PydioVAPP* (ssh access)
* MySQL username: *root* / No password
* MySQL username: *pydio@localhost* / No password

The predefined database created in MySQL is *cells*

## Notes for configuration

### Systemd service

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

### Firewalld service

Firewalld service is active and open two ports:
* 443: **Cells-Enterprise**
* 22: **ssh**

### SELinux

SELinux is running in *permissive* mode
