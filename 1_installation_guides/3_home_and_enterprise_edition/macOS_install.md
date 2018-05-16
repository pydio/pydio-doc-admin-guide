_This guide describes the steps required to have Pydio Cells running on macOS._

## Requirements

### PHP

PHP FPM is required to run the Pydio frontend. To install, run the following commands:

```
brew tap homebrew/dupes
brew tap homebrew/versions
brew tap homebrew/homebrew-php
brew install php72 php72-intl
```

### Database 
*you can skip this step if you already have a database.*

You can use either MySQL (>= 5.6) or MariaDB as your Database Management System. Both are available in Homebrew.

```brew install mysql```
or
```brew install mariadb```

## Installation

### Pydio

Download Pydio Cells Binary on your server/machine using the following command :

```
wget https://download.pydio.com/pub/cells/release/0.9.0/darwin-amd64/cells
chmod +x cells
```

### Port 80 & 443

You can only use these ports if you are connected as root.

By default, Apache is running on macOS, so you need to ensure that it - or no other webservers - is bound to these ports.

To stop the default Apache, you can use : 

```sudo apachectl stop```

To prevent Apache from starting during launch, you may use :

```sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist```

### Database configuration

In this section, we assume you have installed MySql server. Adapt the following steps to your current installation.

```
# Go to mysql mode
sudo mysql -u root
# Create new user and set password
CREATE USER 'cells'@'localhost' IDENTIFIED BY 'cells';
SET password for 'cells'@'localhost' = PASSWORD('your password goes here');
```

## Starting with Pydio

First, give execution rights to the binary. For instance, you can use `sudo chmod u+x <binary>`.

Then, to launch the installer, type:

```./cells install```

A menu will appear.

* **Installation mode** : you can use a browser or CLI to install Pydio Cells.

* **Bind URL** : internal url for PYDIO in most common cases it's `<ip>:<port>`(example : `192.168.0.192:80`) if you are going to use SSL you should put your secure port `443` or else.

* **External URL** : used to access Pydio from outside, usually it will auto fill using the field above and should stay the same.

* **Choose SSL Activation mode** : You can now choose how you're going to activate your SSL, you have 2 choices:
   * **Provide paths to certificate/key files** : if you already have a certificate/key you can use them.
   * **Generate a self-signed certificate** : we will create a self signed certificate for you, be advised this should only be used for staging purposes.


*Subsequent steps are then pretty much the same in the browser or in the CLI.*

1. **Database connection** : put your database informations.

2. **PHP-FPM Detection** : the installer detects your version of php-fpm and if there are missing packages.
If you have a warning because you're using php 5.5.9, don't worry and press next.

3. **Admin User** : Pydio's Admin user informations.

4. **Advanced Settings** : you can skip this part for the time being.

5. **Apply Installation** : you will have progress bar. If you are using the web-based installer, the page will then reload automatically, so don't quit this page or press anything.

To stop Pydio you can press `ctrl + c` and then to start it again use this command
`./cells start`.

## Troubleshooting

* The php-fpm service might not be started, you can look at its status using : `brew services list` and then `brew services start php72` if needed.

* The database ervice might not be started, you can look at its status using : `brew services list` and then `brew services start mysql` if needed.

* You can look at the webserver's error file located in `~/.config/pydio/cells/logs/caddy_errors.log`.
