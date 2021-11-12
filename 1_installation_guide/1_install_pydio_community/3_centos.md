<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP). Time to move to <a href="https://pydio.com/en/docs/administration-guides">Pydio Cells</a>!</span>
</div>

## Install Software Collection, EPEL and Pydio repositories

**EPEL**

    yum install epel-release scl-utils

**CentOS 7** 

    yum install centos-release-scl

**or RHEL 7**

    yum-config-manager --enable rhel-server-rhscl-7-rpms

**Remi's RPM**

    rpm -Uvh https://www.softwarecollections.org/en/scls/remi/php56more/epel-7-x86_64/download/remi-php56more-epel-7-x86_64.noarch.rpm 

**Pydio**

    rpm -Uvh https://download.pydio.com/pub/linux/centos/7/pydio-release-1-1.el7.centos.noarch.rpm

## Installation

At this point, your box contains all dependencies necessary to install Pydio. Hit the following commands to update.

    yum clean all
    yum update

### Installing Pydio Core (first time)

Pydio can be installed by: `yum install pydio-core`

*pydio-core*: contains essential packages with basic functionalities.

you can additionally install some plugins by using command: `yum install pydio-plugin-pluginName`

Or `yum install pydio-all ` to install all community packages of Pydio.

### Upgrading from Pydio 6.4.2

> Warning: If you've installed Pydio before, please backup your sytem or take a snapshoot before upgrade.

Update the whole system

    yum update -y

Upgrade pydio only

    yum update pydio-core

## Post install/upgrade.

This step is required to reconfigure Pydio, apache and php. Because the system now contains multiple version of apache and php, we should
do:

1. Disable default apache and php5.4
2. Enable new apache and php5.6
3. Migrate configuration of php5.4, old apache to new version

Disable default apache:

    systemctl disable httpd
    systemctl stop httpd

Enable new apache:

    systemctl enable httpd24-httpd
    systemctl start httpd24-httpd

Enable php56

    source /opt/rh/rh-php56/enable

Enable httpd24-http

    source /opt/rh/httpd24/enable

Now you can verify the PHP version by typing the command in terminal windows:

    [root@localhost ~]# php -v
    PHP 5.6.5 (cli) (built: Aug 30 2016 13:52:26)
    Copyright (c) 1997-2014 The PHP Group
    Zend Engine v2.6.0, Copyright (c) 1998-2014 Zend Technologies
        with the ionCube PHP Loader (enabled) + Intrusion Protection from ioncube24.com (unconfigured) v6.0.5, Copyright (c) 2002-2016, by ionCube Ltd.
        with Zend OPcache v7.0.4-dev, Copyright (c) 1999-2014, by Zend Technologies

Software Collection use different location than default to deploy software base on its version. And new location for PHP56 is:

Equivalent paths between default and software collection version


|file| default version   |      Softwarecollection version      |
|-|----------|:-------------|
|php.ini| /etc/php.ini |  /etc/opt/rh/rh-php56/php.ini |
|php cli| /usr/bin/php |    /opt/rh/rh-php56/root/usr/bin/php   |
|php module configs| /usr/lib64/php/modules | /opt/rh/rh-php56/root/usr/lib64/php/modules |
|apache module configs| /etc/httpd/conf.modules.d/ | /opt/rh/httpd24/root/etc/httpd/conf.modules.d/ |
|pydio.conf| /etc/httpd/conf.d/pydio.conf/pydio.conf | /opt/rh/httpd24/root/etc/httpd/conf.d/pydio.conf/pydio.conf |


If you upgrade from Pydio 6.4.2, and php.ini was changed, you should change such parameters in new php.ini as well: /etc/opt/rh/rh-php56/php.ini



### Update database

All sql script is store in /usr/share/doc/pydio/sql, you can execute following command to **upgrade** your exited DB

    mysql -u username -p databasename < /usr/share/doc/pydio/sql/pydio.mysql


