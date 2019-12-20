**WARNING: THIS IS WORK IN PROGRESS. NOT YET READY TO BE PUBLISHED** 


_This section gives advices and hints to run Pydio Cells in AWS ecosystem._

In this guide, we explain how to install a vanilla Cells on an `Amazon Linux 2` EC2 instance, step by step.
If you rather want to use another OS, please refer to the corresponding installation guide.

You should also consider directly using the prepared AMI. If such an instance is not available in your AZ, please get in touch with [our support team](mailto:support@pydio.com).

## Prerequisites

### Create an EC2 instance

Based on current requirements for Cells to run correctly, you should choose an instance that has at least 2 vCPU and 4 GB RAM.

- Log in your AWS console, go to `Compute >> EC2 >> Instances` section
- Click on `Launch Instance`
- Select an `Amazon Linux 2 AMI (HVM), SSD Volume Type`
- Choose a `t2.medium` or larger machine
- You probably want to give more storage place to your instance (if you put all your business files in S3, a 50GB storage should be enough, begining with 20GB is OK)
- Choose or create a security group, for a vanilla instance you should enable: 
  - SSH from your own IP
  - HTTP and/or HTTPS from everywhere (80 & 443, source: 0.0.0.0/0,::/0)
- Review and launch
- Choose the correct Key Pair for SSH 

### Login your newly created instance and terminate installation

Once your instance is started, connect to your instance via ssh using the provided public IP address:

```sh
ssh -i /<path to your key pair file>/<name of file>.pem ec2-user@<your public IP>
```

Once logged in, already perform a few pre-install steps:

```sh
# Get up-to-date software distribution 
sudo yum update -y
# install necessary third parties
sudo yum install libcap-ng-utils
```

### Create a dedicated user 

```sh
sudo useradd -m pydio
sudo passwd pydio
```

### Database

The only hard requirement is a running MySQL database server. At the time of writing (Dec. 2019), best option is to use the MariaDB 10.2 that is included in `lamp-mariadb10.2-php7.2` topic.

```sh
# Install the lamp topic using the amazon-linux-extras tool
sudo amazon-linux-extras install lamp-mariadb10.2-php7.2
sudo yum install mariadb-server
sudo systemctl enable mariadb
# remove un-necessary php libraries & services
sudo yum remove php-pdo php-mysqlnd php-cli php-json php-fpm

# Start the server and secure it
sudo systemctl start mariadb
mysql_secure_installation

# Prepare a user and a db for cells
mysql -u root -p -e 'CREATE DATABASE cells;'
# *WARNING* put a real password
mysql -u root -p -e "CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<your-password-here>'";
mysql -u root -p -e "GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost'; FLUSH PRIVILEGES;"

# *For the record only*
# A one-liner to do everything
mysql -u root -p<your password> -e "CREATE DATABASE cells; CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<pydio password>'; GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost'; FLUSH PRIVILEGES;"
# Here is how you might clean your local DB
mysql -u root -p -e "DROP DATABASE cells; DROP USER 'pydio'@'localhost'; FLUSH PRIVILEGES;"
```

## Install Pydio Cells

```sh
# As pydio user, download the latest version
sudo su - pydio
wget https://download.pydio.com/latest/cells/release/{latest}/linux-amd64/cells
chmod u+x cells
```

In this example, we use standard ports for HTTP (80) and HTTPS (443),so you have to give corresponding permissions to the binary file:

```sh
# as the ec2-user
sudo setcap 'cap_net_bind_service=+ep' /home/pydio/cells
```

**Before you start installing, there are two important parameters that you need to understand:**

- **Internal URL**: it defines the interface where the internal webserver of the application is bound. It MUST contain a server name and a port and must be formatted this way: `<ip-or-domain>:<port>`.

- **External URL**: This is the main entry point from the outside world; the address you will communicate to your end-users. It typically  differs from the internal URL when you are behind a reverse proxy or in a container.

For instance, your application runs in a VM that has this IP: 10.0.0.2 in a private LAN behind a reverse proxy that has a public IP and a A DNS record for domain cells.example.com.
Then set INTERNAL_URL to 10.0.0.2:8080 and EXTERNAL_URL to https://cells.example.com (or http).

You can now run the installer:

```sh
# As pydio user
./cells install
```

Follow the short set of instructions. You can also [refer to this page](./cells-installation) to get more details.  
When the installation is done, you might have to stop and restart the application (typically if you have chosen the CLI installer).

```sh
./cells start
```

Note that this is not the preferred way to run Pydio Cells in a production context, see [the following chapters of our documentation](./run-cells-service) to fine tune a production ready instance.

## Troubleshooting

### SELinux is enforced

If, after a successful installation and when you try to navigate to the main application page with your browser, you land on a blank page with following message:

> Access denied.

ensure you have modified SELinux to be in permissive mode.
