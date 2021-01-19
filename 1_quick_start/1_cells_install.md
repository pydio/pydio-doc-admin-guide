_This section will get you up and running in no-time. Should you encounter any issue, please refer to the more detailed installation guides provided in the following chapter._

## Pre-requisites

#### Hardware/OS

For Pydio Cells to run smoothly, you should meet the following requirements:

- **Server Capacity** : 2 Core CPU - 64bit, 4GB RAM, SSD is also recommended for storage.
- **Operating System**: Debian 8/9/10, Ubuntu 18.04 LTS, CentOS 7, MacOS 10.13/11.1, Windows 10
- **Ulimit**: Make sure to set the number of allowed open files greater than **2048**. For production use, a minimum of **8192** is recommended (see `ulimit -n`).

#### MySQL Database

You must have an available MySQL database, along with a privileged user (e.g. `pydio`) to access this DB. Use one of the following link to install the DB:

- [MariaDB version 10.3 and above](https://downloads.mariadb.org/mariadb/repositories)
- [MySQL version 5.7 and above](https://dev.mysql.com/doc/refman/8.0/en/installing.html) (_**except 8.0.22** that has a bug preventing cells to run correctly_).

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

## Configuration

Use the following command to be guided through the basic steps Cells configuration.

```sh
> ./cells configure
```

#### [Mode 1] Web Installer

If you are on a desktop machine, the installer opens a web page with a wizard for you to provide various configuration parameters, including DB connection info and the login/password of the main administrator.

Click on the image below to see a screencast of the installation:

[:image:1_quick_start/installation/web-installer.gif]

After it completes, the server restarts automatically and you are good to go.

#### [Mode 2] Command line Installer

If you are more a shell person, you can perform the same steps using command-line (cli) prompts/answers. 
The image below shows a screencast of the CLI installation :

[:image:1_quick_start/installation/cli-installer.gif]

After configuration is done, start the server with:

```sh
> ./cells start
```

## Login to Cells

Once the start is finished, you are good to go!
You should be able to open your browser on `https://localhost:8080` 
or (`https://<server ip or domain>:8080`) to see the login page. Accept the browser warning about the certificate and use 
the credentials you have just defined to sign in.

## Connection URL 

Cells web server is starting by default on `0.0.0.0:8080`, which means it will listen to all IPs/domains available 
on the machine network interfaces, on port 8080. If the port is busy (already used by another process), it will try other 
options at install time.

A self-signed certificate is also automatically generated for supporting TLS connection out of the box (https).

You can easily change these parameters (pick your own port, use your own TLS certifcate, use Let's Encrypt, etc.):

 - Either at installation time by providing specific flags (`--bind`, `tls_xxx`): see the [man page of the configure command TODO](LINK TO DOC). 
 - Or after first installation by using the dedicated tool `./cells configure sites`:  
see this [dedicated page for more information about the "sites" (TODO)](LINK TO PAGE).