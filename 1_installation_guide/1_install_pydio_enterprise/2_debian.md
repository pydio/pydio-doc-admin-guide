## Get your API Credentials

Our Enterprise Repositories are password-protected via API Key and API Secret. You can find them in your Pydio.com Account, under the "My API Keys" menu:

[:image-popup:1_installation_guide/install_pydio_api_keys.png]

## Debian 9 (stretch)

Configure the pydio repository

    # Pydio Community & Enterprise repositories
    echo "deb https://download.pydio.com/pub/linux/debian/ stretch main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ stretch main" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise
    sudo apt-get install php-xml

## Debian 8 (jessie)

Configure the pydio repository

    # Pydio Community & Enterprise repositories
    echo "deb https://download.pydio.com/pub/linux/debian/ jessie-backports main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ jessie-backports main" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise
    sudo apt-get install php-xml

## Debian 7 (wheezy)

Ensure that wheezy-backports repositories are enabled in `/etc/apt/sources.list` :

    deb http://httpredir.debian.org/debian wheezy-backports main
    deb-src http://httpredir.debian.org/debian wheezy-backports main

Configure the pydio repository as well as its dependencies

    # DotDeb
    echo "deb http://packages.dotdeb.org wheezy-php56 all" > /etc/apt/sources.list.d/dotdeb.list
    echo "deb-src http://packages.dotdeb.org wheezy-php56 all" >> /etc/apt/sources.list.d/dotdeb.list
    wget -qO - http://www.dotdeb.org/dotdeb.gpg | sudo apt-key add -

    # Pydio Community & Enterprise repositories
    echo "deb https://download.pydio.com/pub/linux/debian/ wheezy-backports main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ wheezy-backports main" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https debian-keyring debian-archive-keyring
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise

## Ubuntu 17.10 (artful)

First, configure the pydio repositories

    echo "deb https://download.pydio.com/pub/linux/debian/ artful main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ artful main" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise
    sudo apt-get install php-xml
    
## Ubuntu 17.04 (zesty)

First, configure the pydio repositories

    echo "deb https://download.pydio.com/pub/linux/debian/ zesty main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ zesty main" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise
    sudo apt-get install php-xml

## Ubuntu 16.04 (xenial)

First, configure the pydio repositories

    echo "deb https://download.pydio.com/pub/linux/debian/ xenial main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ xenial main" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise
    sudo apt-get install php-xml

## Ubuntu 14.04 (trusty)

Stop apache if it is running

    sudo service apache2 stop

Install the main PPA for PHP

    sudo apt-get install -yq software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update

Configure the pydio repositories

    echo "deb https://download.pydio.com/pub/linux/debian/ trusty main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ trusty main" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -
    sudo apt-get update

Now, install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise

Enable the correct apache php module - Note that by default, php5 is installed on Ubuntu 14.04, so these instructions are enabling the php5 module version that Pydio is compatible with. If you have already installed php7 or are in the process of setting up php7, then you can invert the parameters :

    a2enmod php5
    a2dismod php7.0

Restart apache

    sudo service apache2 restart

## UPGRADING FROM PREVIOUS VERSION

### Update Database

In version 8, the migration to the latest version of the database is done automatically when first opening Pydio.

In case of a problem during the migration, you still have the possibility to run the database upgrade scripts yourself :

Get the scripts and apply it to your DB as follow:

    wget https://raw.githubusercontent.com/pydio/pydio-core/develop/dist/php/8.0.0.mysql
    mysql -u DB_USER -p DB_NAME < 8.0.0.mysql

Select the correct db type if it's not mysql (`8.0.0.pgsql`, `8.0.0.sqlite`)

Run each upgrade script, starting from your current version to the last. The list of files can be found here:

    https://github.com/pydio/pydio-core/tree/develop/dist/php

### [Debian 7] Manually update Apache configuration

During upgrade, package manager may not have updated the apache configuration. Look inside /etc/apache2/conf.d/, you must see a symbolic link
from pydio.conf to /etc/pydio/apache2.2.conf. Unlink and replace the symlink with /etc/pydio/apache2.conf.

You can test that a shared link (https://yourserver/pydio/public/linkHash) is working.


-----

After that step, you should be able to access pydio through your browser: http://YOUR_IP_ADDRESS/pydio/

[Quick Start](http://pydio.com/en/docs/v7-enterprise/quick-start)
