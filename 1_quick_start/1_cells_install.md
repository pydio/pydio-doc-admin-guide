_This section will get you up and running in no-time. Should you encounter any issue, please refer to the more detailed installation guides provided in the following chapter._

## Pre-requisites

#### Hardware/OS

For Pydio Cells to run smoothly, you should meet the following requirements:

- **Server Capacity** : 2 Core CPU - 64bit, 4GB RAM, SSD is also recommended for storage.
- **Operating System**: Debian 8/9/10, Ubuntu 18.04 LTS, CentOS 7, MacOS High Sierra, Windows 10
- **Ulimit**: Make sure to set the number of allowed open files greater than **2048**. For production use, a minimum of **8192** is recommended (see `ulimit -n`).

#### MySQL Database

You must have an available MySQL database, along with a privileged user (e.g. `pydio`) to access this DB. Use one of the following link to install the DB:

- [MariaDB version 10.3 and above](https://downloads.mariadb.org/mariadb/repositories)
- [MySQL version 5.7 and above](https://dev.mysql.com/doc/refman/8.0/en/installing.html)

As recommended by databases documentations, make sure not to leave the `max_connections` to its default value (151) while going live in production.

#### Cells binary

Download the Pydio Cells binary corresponding to your architecture using one of the following links:

- [Linux Amd64](https://download.pydio.com/latest/cells/release/{latest}/linux-amd64/cells)
- [Mac OSX](https://download.pydio.com/latest/cells/release/{latest}/darwin-amd64/cells)
- [Windows (64bits)](https://download.pydio.com/latest/cells/release/{latest}/windows-amd64/cells.exe)

For the Enterprise Distribution, use these links :

- [Cells Enterprise Linux Amd64](https://download.pydio.com/latest/cells-enterprise/release/{latest}/linux-amd64/cells-enterprise),
- [Cells Enterprise Mac OSX](https://download.pydio.com/latest/cells-enterprise/release/{latest}/darwin-amd64/cells-enterprise),
- [Cells Enterprise Windows (64bits)](https://download.pydio.com/latest/cells-enterprise/release/{latest}/windows-amd64/cells-enterprise.exe)
- **Replace `cells` by `cells-enterprise` in all the following commands.**

On Linux/MacOSX, make sure to make the binary executable using `chmod +x cells`. Create a dedicated user on the server to install and run Cells. On Linux, if you wish to start server on ports 80 (http) or 443 (https), you have to grant a proper permission:

```sh
setcap 'cap_net_bind_service=+ep' cells
```

## Cells Installation

Start Cells in installation mode:

```sh
$> ./cells install
```

#### [Mode 1] Web Installer

If you are on a desktop machine, the installer opens a web page with a wizard for you to provide various configuration parameters, including DB connection info and the login/password of the main administrator.

Click on the image below to see a screencast of the installation:

[:image:1_quick_start/installation/web-installer.gif]

After it completes, the server restarts automatically and you are good to go.

#### [Mode 2] Command line Installer

If you prefer working from your shell, you can perform the same steps using command line prompts/answers. Click on the image below to see a screencast of the installation :

[:image-popup:1_quick_start/installation/cli-installer.gif]

After it completes, restart the server with:

```sh
$> ./cells start
```

_By default Cells is bound on port `8080`_

## Login to Cells

Once the restart is finished, you are good to go! You should be able to open your browser on `https://localhost:8080` or (`https://<server ip or domain>:8080`) to see the login page. Use the credentials you have just specified to login!
