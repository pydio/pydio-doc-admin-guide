
We provide a packaged Cells Enterprise Appliance that follows the OVF standard.

It is an up-to-date CentOS 7 image that contains a preconfigured MariaDB 10.4 and the Cells Enterprise distribution binary, ready to use out of the box.  

Download the [latest OVF image](https://download.pydio.com/latest/cells-enterprise/release/{latest}/ovf/Cells-Enterprise-OVF-{latest}.zip). An md5 file is also available on the same location for integrity verification.

## Requirements

- RAM : 4G
- CPU : 2 vCores
- CPU support: VT-x/AMD-V
- NICs: bridged adaptor

## Setup in Oracle VirtualBox

In this paragraph, we detail the steps to launch the Pydio Cells appliance with Oracle VirtualBox. The process with other hypervisors is very similar.

- Import the image into VirtualBox
- Configure the virtual network interface:
  - Go to `Your Image >> Settings >> Network`, adapter 1 is already bound to a bridged adapter
  - Choose the correct name for the adapter you want to use
  - Click `OK`.
- Start the virtual machine

_Warning:_ even if the correct Name is already shown, click on it and validate, or you won't be able to start the VM (it is a known issue with the VirtualBox UI that is still there in 6.0.14).

## Configuration

Upon boot a service is launched that:

- Verifies the network setting
- Starts the installer

You can finalise the configuration from your web browser. We display in the VM console accessed from your VirtualBox GUI the full correct URL you can use - formatted as `https://<YOUR_VM_IP>`.

> Note: A self-signed certificate is used by default, you have to explicitelly accept it in your browser.

Follow the steps:

- Accept EULA
- Enter your license key
- Accept default DB configuration (except if you do not want to use the embedded DB)
- Choose a password for the admin user
- Launch installation. It can take up to a minute to reach the end.

The installer service is disabled when the configuration terminates successfully.

At this step, you can login to Cells-Enterprise with the credentials you have entered during the setup and verify everything is up and running.

## Notes on configuration

By default, two root paths are used:

- `/var/cells`: Pydio Cells working dir. It contains dynamic configuration (including certificates), data and logs.
- `/opt/pydio`: binaries and additional libraries required to run Cells.

### Predefined accounts

If you ever need to log into the VM system, SSH accounts and technical accounts are created as follow:

| System users        | Password    | Comments   |
| ------------------- | ----------- | ----------- |
| root                | cells       | -        |
| sysadmin            | cells       | Has sudo rights without password |
| pydio               | cells       | Limited user that runs the app |

| MySQL users        | password    |
| ------------------- | --------------- |
| root                | cells       |
| pydio@localhost     | cells       |

The predefined database created in MySQL is *cells*

### Systemd service

**cells** service is running under **pydio** user. The service is **enabled** (a.k.a will automatically restart upon reboot).

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

### SELinux

SELinux is running in *permissive* mode.

### Use of standard HTTP ports

The Cells binary needs specific permission to be allowed to use the standard HTTP ports.

443 (and (=You To In order for Cells to be able to use well known 80 & 443 port, you have to give specific permissions to the binary file.
The OVF you have downloaded is correctly configured and the embedded `cells-enterprise` binary has already these permission set. Yet, if you ever need to update the binary file - typically when you perform an in-app update - you have to re-apply these permission on the new binary file:

```sh
# log as pydio via ssh into the machine
sudo setcap 'cap_net_bind_service=+ep' /opt/pydio/bin/cells-enterprise
```
