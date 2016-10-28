## Get your API Credentials

Our Enterprise Repositories are password-protected via API Key and API Secret. You can find them in your Pydio.com Account, under the "My API Keys" menu:

[:image-popup:1_installation_guide/install_pydio_api_keys.png]

## Debian 8 (Jessie)

Configure the pydio repository

    # Pydio Community & Enterprise repositories
    echo "deb https://download.pydio.com/pub/linux/debian/ jessie-backports main" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ jessie-backports main non-free" > /etc/apt/sources.list.d/pydio.list    
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise

## Debian 7 (Wheezy)

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
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ wheezy-backports main non-free" > /etc/apt/sources.list.d/pydio.list    
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -

Now update all repositories, with https support if not already installed

    sudo apt-get install apt-transport-https debian-keyring debian-archive-keyring
    sudo apt-get update

And finally install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise


## Ubuntu 16.04

First, configure the pydio repositories

    echo "deb https://download.pydio.com/pub/linux/debian/ xenial main universe" > /etc/apt/sources.list.d/pydio.list
    echo "deb https://API_KEY:API_SECRET@download.pydio.com/auth/linux/debian/ xenial main universe" >> /etc/apt/sources.list.d/pydio.list
    wget -qO - https://download.pydio.com/pub/linux/debian/key/pubkey | sudo apt-key add -    
    sudo apt-get update

Now install pydio

    sudo apt-get install pydio
    sudo apt-get install pydio-all
    sudo apt-get install pydio-enterprise

## Ubuntu 14.04

TODO

## UPGRADING FROM PREVIOUS VERSION

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

[Quick Start](http://pydio.com/en/docs/v7-enterprise/quick-start)

