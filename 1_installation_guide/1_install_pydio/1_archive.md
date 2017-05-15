<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 7. Looking for <a href="https://pydio.com/en/docs/v8/install-archive">Pydio 8 docs?</a></span>
</div>

Download the latest stable version from [Download Page](https://pydio.com/en/get-pydio) either as ZIP or TAR.GZ format. Alternatively, you can use our Linux repositories to install Pydio using a package manager on Debian or CentOS Linux flavors.

Using your favorite FTP client or SCP command, upload this package to your webserver, and extract its content to a web-accessible folder (e.g. */var/www/pydio*).

Make sure the *data* path is writeable by the web server. For example :

> `chown -R www-data:www-data /var/www/pydio/data`

Assuming your webserver is configured to serve */var/www/pydio/data*. You can now connect to your server and set up Pydio to suit your needs.

[Quick Start](http://pydio.com/en/docs/v6-enterprise/quick-start)
