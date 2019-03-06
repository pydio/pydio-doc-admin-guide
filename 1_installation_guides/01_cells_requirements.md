Pydio Cells comes as statically linked binaries that do not require specific dependencies to run. However, a fully functional installation will still require some additional software on your system. These are described below.

### Hardware

For Pydio Cells to run smoothly, you should meet the following requirements:

* **RAM** : 4Gb is recommended
* **CPU** : 2 cores minimum
* **HD**  : we recommend you use an SSD as it has faster access time and huge writing/reading speeds.

### Operating System

* **Ulimit**: Make sure to set the number of allowed open files for the binary to something greater than 2K. For production, a minimum of 8K is recommended (see `ulimit -n`).
* **GlibC** : The only hard requirement of Cells binary is using a GLibc library > 2.14. This should be OK on most modern operating systems, this may exclude for example Debian 6.

### Software

Pydio Cells comes with a self-embedded Web server (based on the [Caddy](https://caddyserver.com/docs) project) and does not require any additional Apache or Nginx http services. The backend features were fully rewritten in Golang and the frontend code now only rely on Javascript (more specifically React and Redux). They are also embedded in the binary.

Thus the only remaining hard requirement is the database. At the time of writing, Pydio Cells supports:

* **MySQL** version 5.6 and above.
* **MariaDB** version 10.2 and above.

_Be advised we do not support yet the new **mysql 8 authentication method** right now mysql 8 can only be used with the legacy method with cells for the mean time (it's on our roadmap)_.

### Networking

Pydio Cells webserver may be bound to any port that suits your security rule. However if you need to set it on the standard HTTP (**80**) and HTTPS (**443**) ports, please refer to the following [os-specific sections](/en/docs/cells/v1/os-specific-guides).

**If you wish to use the 0.0.0.0 address you must respect this rule, cells_bind must have this form `cells_bind=0.0.0.0:<port>` and `cells_external=<domain name,address>:<port>` the port is mandatory in both otherwise you will have a grey screen stuck in the loading**

_A private ip is required to run cells as it is used by one of your dependencies, make sure that you have one before running cells, refer to the [troubleshooting](https://pydio.com/en/docs/cells/v1/troubleshooting) in the **Networking** section for hints to work around it._

### Cells Binary

Download the Pydio Cells binary for your OS from the [download page](https://pydio.com/download/) or use the direct links below :

* **[Home Edition](https://download.pydio.com/pub/cells/release/1.4.1/linux-amd64/cells)**
* **[Enterprise Edition](https://download.pydio.com/pub/cells-enterprise/release/1.4.1/linux-amd64/cells-enterprise)**
