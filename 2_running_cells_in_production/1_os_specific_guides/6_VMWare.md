The Pydio Cells image for VMWare is based on CentOS 7. It has been enriched with necessary third party software and pre-configured to provide an easy to run instance of the Cells server out of the box.  
It is known to run in VSphere echosystems and in standalone ESXi hosts.

## Download the package

The latest image for VMWare can be [Downloaded Here](https://download.pydio.com/latest/cells-enterprise/release/{latest}/vmware/Cells-Enterprise-VMWare-{latest}.zip).

An md5 file is also available on the same location for integrity verification.

## Launching a VM

In this guide, we use the simple standalone ESXi web interface to launch a new virtual machine.

Virtual Machine requirements

- RAM : 4G
- CPU : 2 vCores
- CPU support: VT-x/AMD-V
- NICs: bridged adaptor

### Setup and config at the first time

After importing the image into ESXi, you only have to select the correct virtual network interface before launching the machine.  
Go to `Your Image >> Settings >> Network`: Adapter 1 is already bound to a bridged adapter, choose the correct name for the adapter you want to use and click `OK`.

_Warning:_ even if the correct Name is already shown, click on it and validate, or you won't be able to start the VM (it is a known issue with the VirtualBox UI that is still there in 6.0.14).

At first boot, a script is launched to verify network settings and start the installer. You can then interact with a specific Cells service via a web browser to setup and configure your instance.

For your convenience, we display (in the VM console accessed from your VirtualBox GUI) the full correct URL you have to use to access this post installation configuration service - formatted as `https://<DN or IP>/`.

> Note: A self-signed certificate is used by default, ignore the warning message on your web browser.

You just follow steps in web interface to finalize the configuration. All parameters are set by default except the main administrator password.

You see installation progress in your browser page. It can take up to a minute to reach the end.

At this step, you can login to Cells-Enterprise with the credentials you have entered during the setup and verify everything is up and running.

This done, you should restart the virtual machine, another script will finalise the configuration, typically installing and starting Cells as a systemd service.

### Predefined accounts

If you ever need to log into the VM system, SSH accounts and technical accounts are created as follow:

| user                | username        | password    |
| ------------------- | --------------- | ----------- |
| administrative user | root            | cells       |
| user                | pydio           | cells       |
| MySQL root user     | root            | no password |
| MySQL user          | pydio@localhost | no password |

The predefined database created in MySQL is *cells*

## Notes on configuration

By default, two root paths are used:

- `/var/cells`: Pydio Cells working dir. It contains dynamic configuration (including certificates), data and logs.
- `/opt/pydio`: binaries and additional libraries required to run Cells.

### Use of well-known ports

In order for Cells to be able to use well known 80 & 443 port, you have to give specific permissions to the binary file.
The OVF you have downloaded is correctly configured and the embedded `cells-enterprise` binary has already these permission set. Yet, if you ever need to update the binary file - typically when you perform an in-app update - you have to re-apply these permission on the new binary file:

```sh
# log as pydio via ssh into the machine
sudo setcap 'cap_net_bind_service=+ep' /opt/pydio/cells-enterprise
```

### Systemd service

**cells-enterprise** service is running under **pydio** user. The service is **enabled** (a.k.a will automatically restart at reboot).

To manually restart Cells:

```sh
# As pydio user
sudo systemctl restart cells
```

You can consult the output of **cells-enterprise** service by using command:

```sh
sudo journalctl -f
# or to see only cells related log:
sudo journalctl -f -u cells
```

To start/stop the database (MariaDB):

```sh
sudo systemctl start mariadb
```

### Firewalld service

Firewalld service is active and opens three ports:

- 80: **cells-enterprise**
- 443: **cells-enterprise**
- 22: **ssh**

### SELinux

SELinux is running in *permissive* mode.
