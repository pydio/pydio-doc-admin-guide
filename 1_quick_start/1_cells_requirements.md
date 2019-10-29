Pydio Cells ships as a statically linked binary that does not require specific dependencies to run. However, a fully functional installation requires a decent server, a MySQL database and some fine tuning as described below.

### Hardware

For Pydio Cells to run smoothly, you should meet the following requirements:

* **RAM** : 4 GB is recommended
* **CPU** : 2 Cores minimum
* **HD**  : Preferably an SSD

### Operating System

* **Supported Operating systems**: Debian, Ubuntu, CentOS, MacOS, Windows
* **Ulimit**: Make sure to set the number of allowed open files for the binary to something greater than **2048**. For production, a minimum of **8192** is recommended (see `ulimit -n`).

### Database

* **MySQL** version 5.7 and above.
* **MariaDB** version 10.3 and above.
