_This guide describes the steps required to have the Pydio Cells running via an OVF disk image._

[:image-popup:1_installation_guides/logos-os/logo-ovf.png]

## Download OVF Package

The OVH image can be downloaded from:

```sh
https://download.pydio.com/pub/cells-enterprise/release/VERSION/ovf/Cells-Enterprise-OVF-VERSION.zip
```

where "VERSION" is the release version of Pydio Cells. Get for example: [https://download.pydio.com/pub/cells-enterprise/release/1.5.0/ovf/Cells-Enterprise-OVF-1.5.0.zip](https://download.pydio.com/pub/cells-enterprise/release/1.5.0/ovf/Cells-Enterprise-OVF-1.5.0.zip). An md5 file is also available on the same location for integrity verification.

This Zip archive contains the OVF itself plus the following files:

- **EULA**: End-user license agreement
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

After importing the image into VirtualBox, a script will be launched at first boot of the machine. You can then interact with a specific Cells service via a web browser to setup and configure your instance.

For your convenience, we display (in the VM console you can access via virtual box during the script) the full correct URL that you should use to access this post installation configuration service - formatted as https://ipaddress/.

> Note: A self-signed certificate is used, ignore the warning message on your web browser.

You just follow steps in web interface to finalize the configuration. All parameters are set by default except admin's password.

You see installation progress in your browser page. It takes several minute to reach the end.

> Note: Once installation is finished, (and despite what is written on the page) you must manually refresh the page because the self-signed certificate requires a confirmation.

At this step, you can login to Cells-Enterprise with credential you have entered during the setup. It is highly recommended to restart the virtual machine.

### Predefined accounts

If you ever need to log into the VM system, SSH accounts and technical accounts are created as follow:

- Administrative user: *root* / *PydioVAPP* (ssh access)
- User: *pydio* / *PydioVAPP* (ssh access)
- MySQL username: *root* / No password
- MySQL username: *pydio@localhost* / No password

The predefined database created in MySQL is *cells*

## Notes for configuration

### Use of well known port

In order for Cells to be able to use well known 80 & 443 port, you have to give specific permission to the binary files.
The OVF you have downloaded is correctly configured and embedded cells binay these permission set. Yet if you ever need to update the binary file, typically that is what happens under the hood when you perform an update via the Admin settings page, you have to re-apply these permission on the new binary file:

```sh
# log as root via ssh into the machine
cd /home/pydio
setcap 'cap_net_bind_service=+ep' cells
```

### Systemd service

**cells-enterprise** service is running under **pydio** user. You find out all files in /home/pydio/.config/pydio/cells and it is configured to start at boot time, using systemd.

To manually start/stop **cells-enterprise**:

```sh
systemctl start cells
systemctl stop cells
```

You can consult the output of **cells-enterprise** service by using command:

```sh
journalctl -f
# or to see only cells related log:
journalctl -f -u cells
```

To start/stop the database (MariaDB):

```sh
systemctl start rh-mariadb102-mariadb
systemctl stop rh-mariadb102-mariadb
```

### Firewalld service

Firewalld service is active and open two ports:

- 443: **cells-enterprise**
- 22: **ssh**

### SELinux

SELinux is running in *permissive* mode.
