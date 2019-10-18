Pydio Cells comes as statically linked binaries that do not require specific dependencies to run. However, a fully functional installation will still require some additional software on your system. These are described below.

### Hardware

For Pydio Cells to run smoothly, you should meet the following requirements:

* **RAM** : 4 GB is recommended
* **CPU** : 2 cores minimum
* **HD**  : SSD

### Operating System

* **Ulimit**: Make sure to set the number of allowed open files for the binary to something greater than **2048**. For production, a minimum of **8096** is recommended (see `ulimit -n`).

### Database

* **MySQL** version 5.7 and above.
* **MariaDB** version 10.3 and above.
