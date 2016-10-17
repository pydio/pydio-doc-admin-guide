## Install repositories using our release RPM's.

Install EPEL repositories:

> `yum -y install epel-release`

Grab the Release RPM's using the following commands:
> `wget https://download.pydio.com/pub/linux/centos/7/pydio-release-1-1.el7.centos.noarch.rpm`

Install repositories by installing the release RPMs:

> `rpm -i pydio-release-1-1.el7.centos.noarch.rpm`

Update repositories cache `yum update`

## Install Pydio Community Distribution

Now that the repositories are properly configured, you simply have to install pydio using:

> `yum install pydio`

After that step, you should be able to access pydio through your browser: http://YOUR_IP_ADDRESS/pydio/
