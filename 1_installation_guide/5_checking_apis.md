Pydio requires a RewriteRule to redirect all request to */index.php*.  

### Enabling Rewrite Rules

The most important requirement is the webserver ability to support a RewriteRule mechanism, and that the application .htaccess files (or equivalent) are correctly configured.

#### _Enabling ModRewrite on Apache (or similar for IIS & Nginx)_

This is done by activating the module ModRewrite. On most linux server, you can achieve this with the following command

    a2enmod rewrite

You must also make sure that .htaccess instructions are really taken into account. This done by configuring the vhost and making sure that AllowOverride is set to All instead of None. Inside the vhost, check that you see:

    AllowOverride All

For **IIS**, you can adapt this sample configuration [IIS SAMPLE](https://github.com/pydio/pydio-core/blob/develop/core/src/web.config.sample).

For **Nginx**, the location{} tag also contains the according instructions, see [NGINX SAMPLE](https://github.com/pydio/pydio-core/blob/develop/core/src/nginx.conf.sample).

#### _Apache: pydio install path .htaccess rules_

If the installation path of Pydio was webserver-writeable during install, the .htaccess should have been automatically updated.

The variable part of this file is concerning the “RewriteBase”, which reflects **the path of Pydio relative to the DocumentRoot** : if Pydio is installed on http://domain.tld/pydio/, the RewriteBase shall be “/pydio”. If it’s installed on http://domain.tld, should be “/”.

    <IfModule mod_rewrite.c>

    # Make sure to enable RewriteRule on your server, and the the RewriteBase is correctly set.
    # If your install is accessible on https://yourdomain.tld/pydio, RewriteBase should be /pydio.
    # If your install is accessible on https://yourdomain.tld/, RewriteBase should be /.

    RewriteEngine on
    RewriteBase ${APPLICATION_ROOT}         # << THIS LINE SHOULD REFLECT YOUR INSTALLATION PATH
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule (.*) index.php [L]

You can also see in that file some specific instructions that can be uncommented for specific php configurations (fast-cgi, php-fpm)

    #Following lines seem to be necessary if PHP is working
    #with apache as CGI or FCGI. Just remove the #
    #See http://doc.tiki.org/WebDAV#Note_about_Apache_with_PHP_as_fcgi_or_cgi
    RewriteCond %{HTTP:Authorization} ^(.*)
    RewriteRule ^(.*) - [E=HTTP_AUTHORIZATION:%1]

    #Following lines may be necessary for a PHP-FPM setup
    # to make sure that authorization is transmitted.
    # Just remove the # at the beginning of the line
    SetEnvIf Authorization "(.*)" HTTP_AUTHORIZATION=$1

If when testing the API access, you have an infinite authorization loop, try uncommenting one of these lines.

### Other servers: IIS and Nginx

The content of the .htaccess file is only understood by Apache ModRewrite. If you are using another webserver, please refer to the dedicated How-To’s in our knowledge base:

- [Installing Pydio on Windows + IIS](https://pyd.io/configure-applicationpool-for-pydio-in-windows2012-iis8/)
- [Installing Pydio on Nginx](https://pydio.com/en/docs/kb/system/installing-linux-distribution-nginx-0#content)

### _Testing rewrite is working_

Along with the Rewrite mechanism and the HTML tag “BASE”, the GUI can use a routing mechanism to create “fake” URLs: when you switch from one workspace to another, you must see that the URL is updated accordingly in the browser. E.G https://domain.tld/pydio/ws-workspace-slug,https://domain.tld/pydio/ws-otherone, https://domain.tld/pydio/dashboard, etc… Same with the current folder path. This allows the end-user to easily use their back/forward browser button to navigate through their own pydio history.

**The basic check is to enter a workspace (e.g. https://domain.tld/pydio/ws-workspace-slug) and simply reload the page.**

If you encounter a 404 here, this means that the RewriteRule is not correctly configured. If it were correctly working, the htaccess would be saying “transfer all urls starting with /pydio/ws- to /pydio/”.

If you encounter a white screen from startup, you may have a wrong BASE value inside the generated HTML, due to a proxy or wrong path detection on the server. You can verify that using your preferred web-developer tools, looking to the BASE tag in the very head of the html code. The BASE must be pointing to the real root of pydio. For example, if installed on https://domain.tld/my-pydio/, you should see the base value “/my-pydio/” (including trailing slahes). If you have set a wrong Server URL in your Pydio Core Options, this might be interpreted to build a wrong BASE, and this will fail the resources to load. To fix that, simply clear the core.ajaxplorer options from the ajxp_plugin_configs in the DB, and the auto-detection should revert to a correct value.

### Shared links

In Pydio, links are served by default on https://domain.tld/pydio/public/. Unlike previous versions, this is handled directly by the main routing, so it should work out of the box. If you are upgrading from a previous version, the legacy data/public/share.php file should have been modified during upgrade
 process and links will be both accessible on /pydio/data/public/linkhash and /pydio/public/linkhash.

You can change this virtual url part in the Pydio configurations, to serve links e.g. on https://domain.tld/pydio/pub/.

### REST API

The REST API relies as well on the Rewrite rules, and in some case (fcgi setup) on a specific set of htaccess instructions for authorization. You can test this only once you have created a Pydio user and tested you could normally access Pydio with these credentials. Then open a new tab and use the following url, using the same credentials when the browser asks you for user/password: https://domain.tld/pydio/api/my-files/ls/

You should receive an XML result listing the content of the “My Files” default workspace.

#### _Troubleshooting login loops_

When testing the API, if you have an authorization loop (browser is asking for login & password again and again), or you have login issues directly in the PydioSync client, please check the following:

- **Php-FPM or FastCGI:** see above in this article, make sure to uncomment the according lines in the main .htaccess of the application.
- **Using “Session Credentials”** feature to connect to a remote storage: if you are authenticating against an alternative backend like ftp or smb using “Session Credentials”, this may prevent an additional performance layer added for the sync client that use a unique key pair to authenticate devices. Try disabling the authfront.keystore plugin.
