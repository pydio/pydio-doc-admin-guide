## 2.1) Create an account to grab your API Credentials

Our Enterprise Repositories are password-protected via API Key and API Secret. You can find them in your Pydio.com Account, under the "My API Keys" menu:

[:image-popup:1_installation_guide/install_pydio_api_keys.png]


## 2.2) Install Repositories

### CentOS7 / RHEL7: install repositories using our release RPM's.

Install EPEL repositories:

> `yum -y install epel-release`

Grab the Release RPM's using the following commands:
> `wget https://download.pydio.com/pub/linux/centos/7/pydio-release-1-1.el7.centos.noarch.rpm`

> `wget https://API_KEY:API_SECRET@download.pydio.com/auth/linux/centos/7/x86_64/pydio-enterprise-release-1-1.el7.centos.noarch.rpm`
where you should replace API_KEY and API_SECRET by corresponding values retrieved from your pydio.com dashboard as seen in previous step.

Install repositories by installing the release RPMs:

> `rpm -i pydio-release-1-1.el7.centos.noarch.rpm`

> `rpm -i pydio-enterprise-release-1-1.el7.centos.noarch.rpm`

The release package will install a new repo /etc/yum.repos.d/pydio-enterprise: **edit this file** and replace API_KEY and API_SECRET with corresponding values from your pydio dashboard.

Verify /etc/yum.repo.d/pydio-enterprise.repo file to make sure that you have correct baseurl in this format.
baseurl=https://API_KEY:API_SECRET@download.pydio.com/auth/linux/centos/7/x86_64

Update repositories cache `yum update`

### CentOS6 / RHEL6

Pydio 7 requires php version >= 5.5.9 due to security reason and improvement of performance. However, CentOS6/RHEL6 system originally does not support php > 5.4. We should use software collections to be able to keep multiple version of PHP on the same machine.

Install additional repositories for software collections.

> `yum update`

> `yum install epel-release`

Grab the Release RPM's using the following commands:

> `wget https://download.pydio.com/pub/linux/centos/6/pydio-release-1-2.el6.noarch.rpm`

> `rpm -i pydio-release-1-2.el6.noarch.rpm`

> `wget https://API_KEY:API_SECRET@download.pydio.com/auth/linux/centos/6/x86_64/pydio-enterprise-release-1-2.el6.noarch.rpm`

> `rpm -i pydio-enterprise-release-1-2.el6.noarch.rpm`

> `yum update`

Verify /etc/yum.repo.d/pydio-enterprise.repo file to make sure that you have correct baseurl in this format.
baseurl=https://API_KEY:API_SECRET@download.pydio.com/auth/linux/centos/6/x86_64

## 2.3) Install Pydio Enterprise Distribution

Now that the repositories are properly configured, you simply have to install pydio using:

> `yum update`

> `yum install pydio-core` or `yum install pydio-all` # TODO: only one command should be mentionned

> `yum install pydio-enterprise`

After that step, you should be able to access pydio through your browser: http://YOUR_IP_ADDRESS/pydio/


> Note: If you are using CentOS6/RHEL6, it would be necessary to follow extra steps:

> `source /opt/rh/php54/enable`

> Disable old php (5.3): `mv /etc/httpd/conf.d/php.conf /etc/httpd/conf.d/php.conf.old`.

> Restart apache: `service httpd restart`

> The php.ini file is located in: ``/opt/rh/php54/root/etc/php.ini`

[Quick Start](http://pydio.com/en/docs/v6-enterprise/quick-start)

