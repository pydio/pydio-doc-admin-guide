We expose below the specificities of the AMI custom setup. For further details, please refer to the other sections of this [administration guide](https://pydio.com/en/docs/administration-guides).

## Cells Ent AMI on AWS Marketplace

You can find a ready-to-use Amazon Machine Image (AMI) for Cells Enterprise Distribution [on the Amazon Web Services Marketplace](https://aws.amazon.com/marketplace/pp/B08CNGR8ZP).

This appliance is based on [Amazon Linux 2023 (AL2023) OS](https://aws.amazon.com/linux/amazon-linux-2023) and has been enriched with necessary third parties and configuration to provide an easy to run instance of the Cells server out of the box.

## Quick Start

If you want to give a first quick glance at Cells with no hassle, just launch the AMI on a `t2-medium` EC2 instance, with no user data and no extra mounted volume.

Define some security groups so that the instance:

- Can reach the internet (to download updates)
- Can be reached on port 443
- Can be reached via SSH

_Note: your instance must at least have **one network interface that is on a private network**, with an IP address that is in a well-known private range (e.g 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 for IPv4). It is the case by default on most regions_.

You will have a ready to install instance at `https://<ec2-public-IP>` after a couple of minutes.

You can quickly finalise the installation by:

- Accepting EULA
- Pasting your licence key
- Use embedded local db that is already configured
- Define an admin and leave advanced parameters untouched.

## Users

The Pydio Cells AMI comes with 3 default users:

- **ec2-user**: the default Amazon user that is to be used to connect using the key pair you define when launching a new instance. She has sudo rights without password for all commands.
- **pydio**: the user that launches the Cells server. She has no sudo rights and no pre-defined password. You should use this user when you locally use the `cells` command line interface.
- **root**: the root user. You normally do not have to log as this user.

## Services

- **install-cells.service**: enabled when you instantiate a new Cells Enterprise AMI, this service launches the installation process. After successful configuration of your instance, it starts and enables the Cells service. It then disables itself before exiting.
- **cells.service**: runs the cells server as a systemd daemon. The `install-cells.service` enables and starts this service when finalising the install.

## Tree structure

They are 2 main paths that are used by Cells:

- `/opt/pydio`:
  - `/opt/pydio/bin`: Pydio Cells binary
  - `/opt/pydio/conf`: Installation configuration files
- `/var/cells`: The application working directory with: runtime configuration, logs, technical persistence and, by default, your data.  

If you provide an additional EBS volume when you launch the AMI (see installation instructions), it is automatically mounted at this location before install.

## Database

A MySQL DB is the only hard requirement to run a Pydio Cells instance.  
As a convenience, the Pydio Cells AMI embbed an installed and configured MariaDB 10.5 server that is provided by the Amazon Linux repositories.

There are 2 users `root` and `pydio`, all other configurations follow the `mysql_secure_installation` best practices.  
There is one default `cells` DB, on which the `pydio` user has all privileges.

You might use this database during your test phase. Before going live, it is recommanded to either:

- Rather use an RDS instance
- Change the passwords for both users

_Hint: default mysql password for pydio user is `cells`, root user has no password_

## Network configuration

By default, the application is installed using the "self-signed certificate" mode.

It is recommended to run the Cells server behind a reverse proxy that exposes a valid TLS certificate when in production.

If you do not provide a valid registered FQDN via "user data" upon launch, it is then configured using the Amazon automatically provided public IP address and the frontend is publicly available at `https://<your-instance-public-ip-address>`.
