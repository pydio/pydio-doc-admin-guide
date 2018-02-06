Pydio code is continuously audited by security experts, and should be resistant to the most common web attacks. But at some point, Pydio cannot fix holes created by a wrong server configuration. Here are some basic recommendations for securing your Pydio installation.

### Protect your folders from direct web access

Under the main Pydio installation folder, the following folders contents must be hidden from the web server. It is by default the case if you are using Apache, as **.htaccess files** are part of the distribution.

+ **pydio_install/conf**
+ **pydio_install/data/[all subfolders except “public”].**  data/public/ is the default container for the “shared links” public files.

**Note:**  concerning the .htaccess files under Apache, be sure to AllowOverride of the Limit directives on your web server (contact your Webmaster).

If you can, do not use the default “files” folder placed inside the distribution, but create a repository pointing to a folder outside your web “document root”.

[Read more](https://pydio.com/en/docs/kb/security/permission-pydio%E2%80%99s-filesfolders)

### Basic security rules

HTTPS usage is recommended, but you have to configure your server for that, it cannot be done automatically by Pydio.

Always use strong passwords. There is a password minimum length option that is set to 8 characters by default.

###Check for updates

Security issues are always released with high priority, use the integrated upgrade tool to check if updates are available and apply them!
