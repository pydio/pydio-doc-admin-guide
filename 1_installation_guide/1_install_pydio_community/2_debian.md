## Debian 9 (Stretch)

Configure the pydio repository

    # Pydio Community repositories
    echo "deb https://download.pydio.com/pub/linux/debian/ stretch main" > /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install php-xml

## Debian 8 (Jessie)

Configure the pydio repository

    # Pydio Community repositories
    echo "deb https://download.pydio.com/pub/linux/debian/ jessie-backports main" > /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install php-xml

## Debian 7 (Wheezy)

Ensure that wheezy-backports repositories are enabled in `/etc/apt/sources.list` :

    deb http://httpredir.debian.org/debian wheezy-backports main
    deb-src http://httpredir.debian.org/debian wheezy-backports main

Configure the pydio repository as well as its dependencies

    # DotDeb
    echo "deb http://packages.dotdeb.org wheezy-php56 all" > /etc/apt/sources.list.d/dotdeb.list
    echo "deb-src http://packages.dotdeb.org wheezy-php56 all" >> /etc/apt/sources.list.d/dotdeb.list
    wget -qO - http://www.dotdeb.org/dotdeb.gpg | sudo apt-key add -

    # Pydio Community repositories
    echo "deb https://download.pydio.com/pub/linux/debian/ wheezy-backports main" > /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https debian-keyring debian-archive-keyring
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
   
## Ubuntu 17.10 (artful) 

First, configure the pydio repository as well as its dependencies

    echo "deb https://download.pydio.com/pub/linux/debian/ artful main" > /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now, install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install php-xml
    
## Ubuntu 17.04 (zesty) 

First, configure the pydio repository as well as its dependencies

    echo "deb https://download.pydio.com/pub/linux/debian/ zesty main" > /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now, install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install php-xml

## Ubuntu 16.04 (xenial) 

First, configure the pydio repository as well as its dependencies

    echo "deb https://download.pydio.com/pub/linux/debian/ xenial main" > /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now, install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install php-xml

## Ubuntu 14.04 (trusty)

Stop apache if it is running

    sudo service apache2 stop

Install the main PPA for PHP

    sudo apt-get install -yq software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update

Configure the pydio repository as well as its dependencies

    echo "deb https://download.pydio.com/pub/linux/debian/ trusty main" > /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now, install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all

Enable the correct apache php module - Note that by default, php5 is installed on Ubuntu 14.04, so these instructions are enabling the php5 module version that Pydio is compatible with. If you have already installed php7 or are in the process of setting up php7, then you can invert the parameters :

    a2enmod php5
    a2dismod php7.0

Restart apache

    sudo service apache2 restart

## UPGRADING FROM PREVIOUS VERSION

### Pydio Updater Plugin

In pydio 8, the brand new updater plugin eases the transition to the latest version. You just have to install this package, and the functionality will check and perform the database upgrade for you when first connecting to the Pydio application

    sudo apt-get install pydio-plugin-action.updater

### Update Database

If you are updating from version 6, you will have to manually update the database. Get the upgrade script and apply it to your DB as follow:

    wget https://raw.githubusercontent.com/pydio/pydio-core/develop/dist/php/7.0.1.mysql
    mysql -u DB_USER -p DB_NAME < 7.0.1.mysql

Select the correct db type if it's not mysql (`7.0.1.pgsql`, `7.0.1.sqlite`)

### [Debian 7] Manually update Apache configuration

During upgrade, package manager may not have updated the apache configuration. Look inside /etc/apache2/conf.d/, you must see a symbolic link
from pydio.conf to /etc/pydio/apache2.2.conf. Unlink and replace the symlink with /etc/pydio/apache2.conf.

You can test that a shared link (https://yourserver/pydio/public/linkHash) is working.


-----

After that step, you should be able to access pydio through your browser: http://YOUR_IP_ADDRESS/pydio/

[Quick Start](http://pydio.com/en/docs/v8/quick-start)
