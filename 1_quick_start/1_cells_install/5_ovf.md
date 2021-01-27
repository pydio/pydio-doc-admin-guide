
We provide a packaged Cells Enterprise Appliance that follows the OVF standard.

It is an up-to-date CentOS 7 image that contains a preconfigured MariaDB 10.4 and the Cells Enterprise distribution binary, ready to use out of the box.  

Download the [latest OVF image](https://download.pydio.com/latest/cells-enterprise/release/{latest}/ovf/Cells-Enterprise-OVF-{latest}.zip). An md5 file is also available on the same location for integrity verification.

## VM Requirements

- RAM : 4G
- CPU : 2 vCores
- CPU support: VT-x/AMD-V
- NICs: bridged adaptor

## Setup in Oracle VirtualBox

We detail here the steps to launch the Pydio Cells appliance with Oracle VirtualBox. The process with other hypervisors is very similar.

- Import the image into VirtualBox
- Configure the virtual network interface:
  - Go to `Your Image >> Settings >> Network`, adapter 1 is already bound to a bridged adapter
  - Choose the correct name for the adapter you want to use
  - Click `OK`.
- Start the virtual machine

_Warning:_ even if the correct Name is already shown, click on it and validate, or you won't be able to start the VM (it is a known issue with the VirtualBox UI that is still there in 6.0.14).

## Configure

We display the URL address of the installer when you start your VM. Check in the console that is accessed from your VirtualBox GUI.

A self-signed certificate is used by default, you have to explicitelly accept it in your browser.

Follow the steps:

- Accept EULA
- Enter your license key
- Accept default DB configuration (except if you do not want to use the embedded DB)
- Choose a password for the admin user
- Launch installation. It can take up to a minute to reach the end.

The installer service is disabled when the configuration terminates successfully.

At this step, you can login with the credentials you have defined and verify everything is up and running.

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

Firewalld service is active and opens three ports:

- 80: **cells-enterprise**
- 443: **cells-enterprise**
- 22: **ssh**
