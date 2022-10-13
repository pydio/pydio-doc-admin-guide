The Pydio Cells image for VMWare is based on Rocky Linux 9. It has been enriched with necessary third party software and pre-configured to provide an easy to run instance of the Cells server out of the box.  
It is known to run in VSphere ecosystems and in standalone ESXi hosts.

Download the [latest VMWare image](https://download.pydio.com/latest/cells-enterprise/release/{latest}/vmware/Cells-Enterprise-VMWare-{latest}.zip). An MD5 file is also available at the same location for integrity verification.

## Minimal VM Requirements

- RAM : 4G
- CPU : 2 vCores
- CPU support: VT-x/AMD-V
- NICs: bridged adaptor

## Setup a VM

In this guide, we use the simple standalone ESXi web interface to launch a new virtual machine:

- Log in your web interface
- Go to the `Virtual Machines` section
- Click on Create/Register VM

A creation wizard pops up.

- In `Select creation type`, choose `Deploy a virtual machine from an OVF or OVA file`
- In second page, choose a name and upload **both** the `*.ovf` and the  `*-disk1.vmdk` files
- Choose correct network and disk provisioning
- Click on `Finish` and wait until creation is complete
- Start the virtual machine

## Configure

We display the URL address of the installer when you start your VM. Check the console in the VirtualBox GUI.

A self-signed certificate is used by default, you have to explicitelly accept it in your browser.

Follow the steps of the wizard to finalize your setup. All parameters are set by default except the main administrator password. When it's done, the installer saves everything, starts the `cells` service and exits. It can take up to a minute.

At this step, you can log in the app with the credentials you have just defined and check everything is up and running.


## Configuration Details

### Predefined user accounts

If you need to log into the system, the `ssh` service is enabled on port `22` and following users have been created:

| System users        | Password    | Comments   |
| ------------------- | ----------- | ----------- |
| root                | cells       | -        |
| sysadmin            | cells       | Has sudo rights without password |
| pydio               | cells       | Limited user that runs the app |

To connect to the default *cells* database in MariaDB, you have these users:

| MySQL users        | password    |
| ------------------- | --------------- |
| root                | cells       |
| pydio@localhost     | cells       |

### File Layout

- `/var/cells`: Pydio Cells working directory. It contains business and technical data.
- `/opt/pydio`: binaries and additional libraries required to run Cells.

### Systemd service

**cells** service is running under **pydio** user. The service is **enabled**: it will automatically restart upon reboot.

Useful `systemd` commands, as `sysadmin` user:

```sh
# Manually restart the service
sudo systemctl restart cells
# See Pydio Cells logs
sudo journalctl -f -u cells -S -1h
# start/stop the database (MariaDB)
sudo systemctl start mariadb
```

### Firewalld service

We recommand to use the provided appliance in your private network, behind enterprise grade firewall and reverse proxy. Thus, the firewall service in Cells appliance is disabled and stopped by default.
