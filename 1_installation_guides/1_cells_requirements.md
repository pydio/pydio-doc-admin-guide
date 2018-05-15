Pydio Cells comes as statically linked binaries that do not require specific dependencies to run. However, a fully functional installation will still require some additional software on your system. These are described below.

### Hardware

For Pydio Cells to run smoothly you should meet the following requirements:

* **RAM** : 4Gb is recommended
* **CPU** : 2 cores minimum
* **HD**  : we recommend you to use an SSD as it has faster access time and huge writing/reading speeds.

### Operating System

* **Ulimit**: Make sure to set the number of allowed open files for the binary to something greater than 2K, for production use a limit of 8K is recommended (see `ulimit -n`).
* **GlibC** : The only hard requirement of Cells binary is using a GLibc library > 2.14. This should be ok on most modern operating systems, this may exclude for example Debian 6.

### Software

#### Php-FPM

Pydio Cells comes with a self-embedded Web server (based on the [Caddy](https://caddyserver.com/docs) project) and does not require any additional Apache or Nginx http services. The backend features were fully rewritten in Golang and are also embedded in the binary, but the Frontend code still contains some PHP and a fastcgi connection must be available between pydio binary and php-fpm.

We recommend the usage **PHP 7+** for its overall better performances, but you can still use Pydio with PHP 5.5.9. Pydio Cells frontend will also require the following php extensions along with php:

* **php-gd**
* **php-intl**
* **php-dom**
* **php-curl**

#### MySQL Database

At the moment Pydio Cells supports:

* **MySQL** version 5.6 and above.
* **MariaDB** version 10 and above.

### Networking

Pydio Cells webserver maybe bound to any port that suits your security rule. However, if it is exposed directly to the outside world, it will probably need **80** or **443** ports to be available to serve on standard HTTP/HTTPS ports. See the following os-specific 
[sections](/en/docs/cells/v1/os-specific-guides) to configure the service properly.

### Cells Binary

You can get the cells binary from our public servers [here](https://download.pydio.com/pub/)
or you can use the links below :

* **[Home Edition](https://download.pydio.com/pub/cells/release/0.9.1/linux-amd64/cells)**
* **[Enterprise Edition](https://download.pydio.com/pub/cells-enterprise/release/0.9.1/linux-amd64/cells-enterprise)**
