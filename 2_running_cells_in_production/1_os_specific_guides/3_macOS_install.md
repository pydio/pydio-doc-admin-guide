_This guide describes the steps required to have Pydio Cells running on macOS_.

[:image:2_running_cells_in_production/logos-os/logo-mac.png]

## Requirements

### Database

*You can skip this step if you already have a database*.

You can use either MySQL (>= 5.7) or MariaDB (>= 10.3) as your Database Management System. Both are available in Homebrew.

`brew install mysql` or `brew install mariadb`

## Installation

### Pydio

Download Pydio Cells Binary on your server/machine using the following command:

```sh
# Use this url as is, it will be resolved automatically to latest version

wget https://download.pydio.com/latest/cells/release/{latest}/darwin-amd64/cells
chmod +x cells
```

### Port 80 & 443

You can only use these ports if you are connected as an Admin User or root.

By default, Apache is running on macOS, so you need to ensure that it - or no other webservers - is bound to these ports.

To stop the default Apache, you can use:

```sudo apachectl stop```

To prevent Apache from starting during launch, you may use:

```sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist```

### Database configuration

In this section, we assume you have installed MySql server. Adapt the following steps to your current installation.

```sh
# Go to mysql mode
sudo mysql -u root
# Create new user and set password
CREATE USER 'pydio'@'localhost' IDENTIFIED BY 'your password goes here';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
FLUSH PRIVILEGES;
```

## Starting with Pydio

First, give execution rights to the binary. For instance, you can use `sudo chmod u+x <binary>`.

Then, to launch the installer, type:

```sh
./cells install
```

**Before you start installing here's two of the most important parameters that you need to understand:**

```conf
INTERNAL_URL : address where the application http server is bound to. It MUST contain a server name and a port.

EXTERNAL_URL : url the end user will use to connect to the application.

Example:

If you want your application to run on the localhost at port 8080 and use the url mycells.mypydio.com, then set INTERNAL_URL to localhost:8080 and EXTERNAL_URL to http://mycells.mypydio.com (or https).
```

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again, just run:

```sh
./cells start
```

## Troubleshooting

- The database service might not be started, you can look at its status using : `brew services list` and then `brew services start mysql` if needed.
- You can look at the webserver's error file located in `~/.config/pydio/cells/logs/caddy_errors.log`.
