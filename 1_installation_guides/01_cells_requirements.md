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

#### PHP-FPM

Pydio Cells comes with a self-embedded Web server (based on the [Caddy](https://caddyserver.com/docs) project) and does not require any additional Apache or Nginx http services. The backend features were fully rewritten in Golang and are also embedded in the binary, but the Frontend code still contains some PHP and a fastcgi connection must be available between Pydio Cells binary and php-fpm.

We recommend the usage **PHP 7+** for its overall better performances, but you can also use Pydio Cells with PHP >= 5.5.9. Pydio Cells frontend also requires the following php extensions :

* **php-gd**
* **php-intl**
* **php-dom**
* **php-curl**

#### MySQL Database

At the moment Pydio Cells supports:

* **MySQL** version 5.6 and above.
* **MariaDB** version 10.2 and above.

### Networking

Pydio Cells webserver may be bound to any port that suits your security rule. However if you need to set it on the standard HTTP (**80**) and HTTPS (**443**) ports, please refer to the following [os-specific sections](/en/docs/cells/v1/os-specific-guides).

### Cells Binary

Download the Pydio Cells binary for your OS from the [download page](https://pydio.com/download/) or use the direct links below :

* **[Home Edition](https://download.pydio.com/pub/cells/release/1.0.4/linux-amd64/cells)**
* **[Enterprise Edition](https://download.pydio.com/pub/cells-enterprise/release/1.0.4/linux-amd64/cells-enterprise)**
