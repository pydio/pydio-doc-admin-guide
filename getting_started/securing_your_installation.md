AjaXplorer was tested and corrected with the help of security experts, and should be resistant to the common web attacks (csrf, etc, see the 3.2 release note). But at some point, AjaXplorer cannot fix holes created by a wrong server configuration. Thus here are some basic recommandations for securing your AjaXplorer installation.

### Protect your folders from direct web access
Under the main AjaXplorer installation folder, the following folders contents must be hidden from the web server. It is by default the case if you are using Apache, as **.htaccess** files are part of the distribution.

+ **ajaxplorer_install/conf**
+ **ajaxplorer_install/data/[all subfolders except “public”]**, that is the default container for the “shared links” public files.

**Note**:  concerning the .htaccess files under Apache, be sure to allow override of the Limit directives on your web server (contact your Webmaster).

If you can, do not use the default “files” folder placed inside the distribution, but create a repository pointing to a folder outside your web “document root”.

Read more: https://pyd.io/permission-for-pydios-filesfolders/

### Basic security rules
HTTPS usage is recommended, but you have to configure your server for that, it cannot be done automatically by AjaXplorer.

Always use strong passwords. There is a password minimum length option that is set to 8 characters by default.

### Check for updates
Security issues are always released with high priority, use the integrated upgrade tool to check if updates are available and apply them!