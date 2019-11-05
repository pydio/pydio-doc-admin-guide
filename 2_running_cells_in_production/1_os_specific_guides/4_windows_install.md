_This guide describes the steps required to have Pydio Cells running on Windows_

## Install Cells on Windows 10

### Install a Database

- [MySQL 8.0](https://dev.mysql.com/doc/refman/8.0/en/windows-installation.html)
- [MariaDB 10.4](https://mariadb.org/download/)

### Install Cells

Download the Pydio Cells executable [Download Server](https://download.pydio.com/latest/cells/release/{latest}/windows-amd64/cells.exe).

Open your terminal (powershell by default on windows) then proceed to install with the following command :

- `.\cells.exe install`

> Note, on powershell (terminal) the keys to navigate are H-J-K-L ( J = UP, K = DOWN).

The binary will ask you to choose a mean of installation :

- **browser based**: will open a browser tab with an intuitive installer.
- **command line interface**: for advanced users, pretty straight forward.

Then you have 2 important settings:

- **INTERNAL_URL :** address where the application http server is bound to. It MUST contain a server name and a port.
- **EXTERNAL_URL :** url the end user will use to connect to the application.

There are also multiple ways to add SSL to your installation:
- **SELF_SIGNED :** creates a self_signed certificate.
- **LETS_ENCRYPT :** automagically generates a valid certificate for a domain ([see lets encrypt site](https://letsencrypt.org/))
- **Provide your own certificate :** pass your already generated certificates.

provide your database informations and you are good to go.


Once the installation is done the folder containing your data and settings is located under %APPDATA%.

> To display the folder, this might require enabling the setting, display **hidden files/folders**

**Data Location** : `C:\Users\pydio\AppData\Roaming\Pydio\cells`