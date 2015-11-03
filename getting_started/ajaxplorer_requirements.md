## Core Installation
### Basic requirements
For the most common case, AjaXplorer only requires a web-server equiped with **PHP 5.1** or later, along with the DomXML , MCrypt and GD extensions. They are most generally active by default on a standard php installation. Plugins may require additional extensions / libraries to be available on the server.

The webserver software can be the one of your choice : AjaXplorer is known to be running on Apache, Nginx, Lighttpd, IIS. However, being the more wide-spread and tested in many different configurations, Apache is recommended for a production environnement.

### Recommended hardware
This may highly vary, depending your number of users and volume of documents. The application is running on many shared hosting where you are generally allocated few memory and virtual cores, as well as embedded devices like NAS with few CPU, but if possible, it is always recommanded to oversize the CPU and memory to provide the best user experience.

Thus, if you can provide a dedicated server, starting with a **2GHz dual-core with 4Go of RAM should be more than enough** for basic needs (up to 50 users). Uploads are known to be long and CPU-consuming, thus if you know that your users will frequently require big files uploads, you should size the CPU accordinly.

 

## Plugins Requirements
Many plugins have their own dependancies to external libraries, either as a PHP Extension, and PEAR library, or a given utilitary that must be installed on the server and accessible via command line. In the later case, there are generally common tools that can be installed directly on Linux distribution using the package manager (apt / yum), but their Windows version generally exists and as been tested succesfully as well.

| Name           | Identifier     | PHP Extension | External (PEAR or command line)                        |
|----------------|----------------|---------------|--------------------------------------------------------|
| Email Viewer   | editor.eml     |               | Mail_mimeDecode                                        |
| MySQL Driver   | access.mysql   | mysql         |                                                        |
| SFTP Driver    | access.sftp    | ssh2          |                                                        |
| Samba Driver   | access.smb     |               | smbclient via command line                             |
| Office Docs    | editor.zoho    | openssl       |                                                        |
| PDF Viewer     | editor.imagick |               | Imagick via command line                               |
| Exif extractor | meta.exif      | exif          |                                                        |
| Subversion     | meta.svn       |               | svn via command line                                   |
| Git            | meta.git       |               | git via command line + Pear VersionControl_Git library |
| Dropbox        | access.dropbox |               | HTTP_OAuth                                             |
| Mailbox        | access.mailbox | imap          |                                                        |
| WebDAV Client  | access.webdav  |               | HTTP_WebDAVClient                                      |
| S3 Access      | access.s3      |               | AWS Sdk for PHP                                        |