There is 2 ways to install Pydio on your server:

### 1/ Install from archive

Download the latest stable version from [Download Page](https://pyd.io/download), either as ZIP or TAR.GZ format. Alternatively, you can use our Linux repositories to install Pydio using a package manager on Debian or CentOS Linux flavors.

Using your favorite FTP client or SCP command, upload this package to your webserver, and extract its content to a web-accessible folder (e.g. */var/www/pydio*).

Make sure the *data* path is writeable by the web server. For example :

> `chown -R www-data:www-data /var/www/pydio/data`

### 2/ Install from RPM / DEB

#### 2.1/ Create an account to grab your API keys

Our Enterprise Repositories are password-protected by an API Key / API Secret credentials. These are provided automatically when you have a valid Pydio Enterprise License. For this reason, you first need to register and at least claim your free 10 users license in your Pydio.com Customer Dashboard. Your license will be instantly provisioned, so it's just a matter of minutes. Once you have it, find the "Api Key / Secret" at the bottom of the license page in your Dashboard:
[:image-popup:1_installation_guide/install_pydio_api_keys.png]



#### 2.2/ Install Repositories

##### Debian 8: install public key and repositories

Install apt-transport-https package:

> `sudo apt-get install apt-transport-https`

Install our public key (used to sign the packages) using the following command:

> `wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -`

Then edit /etc/apt/sources.list and add the following lines

> `deb https://download.pydio.com/pub/linux/debian/ jessie-backports main`

> `deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ jessie-backports main non-free`

where you would replace API_KEY / API_SECRET by the values retrieved in the previous step from your pydio.com account.

Now update your repositories list by running:

> `sudo apt-get update`

##### CentOS7 / RHEL7: install repositories using our release RPM's.

Install EPEL repositories:

> `yum -y install epel-release`

Grab the Release RPM's using the following commands:
> `wget https://download.pydio.com/pub/linux/centos/7/pydio-release-1-1.el7.centos.noarch.rpm`

> `wget https://API_KEY:API_SECRET@download.pydio.com/auth/linux/centos/7/x86_64/pydio-enterprise-release-1-1.el7.centos.noarch.rpm`
where you would replace API_KEY / API_SECRET by the values retrieved in the previous step from your pydio.com account.

Install repositories by installing the release RPMs:

> `rpm -i pydio-release-1-1.el7.centos.noarch.rpm`

> `rpm -i pydio-enterprise-release-1-1.el7.centos.noarch.rpm` The latest will install a new repo /etc/yum.repos.d/pydio-enterprise: **edit this file** and replace API_KEY/API_SECRET with your values.

Update repositories cache `yum update`

#### 2.3/ Install Pydio Enterprise Distribution

Now that the repositories are properly configured, you simply have to install pydio using either:
> `apt-get install pydio-enterprise`

or

> `yum install pydio-enterprise`

After that step, you should be able to access pydio through your browser: http://YOUR_IP_ADDRESS/pydio/
