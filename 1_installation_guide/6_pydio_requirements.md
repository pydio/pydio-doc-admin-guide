## Core Installation

### LAMP stack or equivalent

#### Web Server
The webserver software can be the one of your choice : Pydio is known to be running on **Apache**, **Nginx**, **Lighttpd** or **IIS** on Windows. Being the more wide-spread and tested in many different configurations, Apache is recommended for a production environment if you are not already familiar with other software. Please note that *we do not support Apache on Windows*.

#### Database

Pydio can run on three types of databases : MySQL/MariaDB, PostgreSQL or Sqlite. The latter is not recommended for production environment but can be useful for testing or quick simple deployments.

#### PHP

Pydio requires **PHP 5.5.9 or later**. Recommended version is **PHP 7.0**, particularly on Windows as there is now an official 64bit build. 

The following PHP extensions are required as well for the core pydio installation: 

- Mbstring, gd, dom-xml, intl
- Mysql, PostgreSQL or Sqlite php extension for connecting to the database
- OpenSSL or Mcrypt. The latter will be deprecated in next versions of PHP so you should prefer using Open SSL. 
- For Enterprise Distribution, IonCube Loaders must be configured as a zend_extension in your php.ini. Please see https://ioncube.com/loaders.php to pick the right version for your OS / PHP Version.
- Opcache or equivalent Opcode Caching extension

#### Additional Recommended setup:

- A working Command Line PHP (CLI) runnable by webserver will allow Pydio to launch commands in background and make the 
user experience more fluid.
- PHP APC/APCu extension or an additional Redis Server will allow Pydio to cache many data in-memory and greatly boost the performances as well.

Some plugins may require additional extensions / libraries to be available on the server (see below).

### Networking

It is highly recommended to server the Pydio software using the HTTPS protocol to make sure that all communications between server and client are encrypted. If you do not own already an SSL Certificate for your domain, you can check the Let's Encrypt tools to easily secure your server.

Pydio thus requires **port 443** to be open (or 80 if you still want to run on HTTP). To be able to use the web-socket feature provided by Pydio Booster, you must also allow another port, by default **8090/TCP**. You can change this port number in the configuration.

### Hardware

This may highly vary, depending your number of users and volume of documents. The application is running on many shared hosting where you are generally allocated few memory and virtual cores, as well as embedded devices like NAS with few CPU, but if possible, it is always recommended to oversize the CPU and memory to provide the best user experience.

Thus, if you can provide a dedicated server, starting with a **2GHz dual-core with 2Go of RAM should be more than enough** for basic needs (up to 100 users). Uploads are known to be long and CPU-consuming, thus if you know that your users will frequently require big files uploads, you should size the CPU accordingly.

## Other Plugins Requirements

Many plugins have their own dependencies to external libraries, either as a PHP Extension, and PEAR library, or a given utility that must be installed on the server and accessible via command line. In the later case, there are generally common tools that can be installed directly on Linux distribution using the package manager (apt / yum), but their Windows version generally exists and as been tested successfully as well.

| **Name**       | **Identifier** | **PHP Extension** | **External (PEAR or command line)**                    |
|----------------|----------------|-------------------|--------------------------------------------------------|
| Email Viewer   | editor.eml     |                   | Mail_mimeDecode                                        |
| SFTP Driver    | access.sftp    | ssh2              |                                                        |
| Samba Driver   | access.smb     |                   | smbclient via command line                             |
| Office Docs    | editor.zoho    | openssl           |                                                        |
| PDF Viewer     | editor.imagick |                   | Imagick via command line                               |
| Exif extractor | meta.exif      | exif              |                                                        |
| Git            | meta.git       |                   | git via command line                                   |
| Mailbox        | access.mailbox | imap              |                                                        |
