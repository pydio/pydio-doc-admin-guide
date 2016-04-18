Here are some common mistakes you might encounter when you try to install Pydio and a way to fix them.

All examples are with the latest [Pydio-core 6.4.1](https://pydio.com/en/community/releases/pydio-core/pydio-core-641-bugfix-release) with the recommend stack ( Debian - Apache2 - MySQL ).

##Permission issue:

After you put your pydio in your /var/www folder you go to your web browser and you will probably see this:

[:image-popup:troubleshooting_install_pydio/troubleshooting_forgotten.png]

Pydio need to have some access right to read and/or write files. Just run:
`sudo chown -r www-data:www-data /var/www/pydio/data`

##Diagnostic tool issue:

After refreshing your web browser, you should can see this Pydio Diagnostic Tool:

[:image-popup:troubleshooting_install_pydio/troubleshooting_diagnostic_tool.png]

Instead of:

[:image-popup:troubleshooting_install_pydio/troubleshooting_diagnostic_tool_good.png]

###Security breach issue:

So, we have several problems here; first, the "Security Breach" problem is when your data folder is not correctly protected by your apache server. Go to your apache2 folder and edit the "apache2.conf" ( /etc/apache2/apache2.conf ).

You need to edit your conf because from now, your apache2 server ignore all .htaccess files which are in /var/www :

[:image-popup:troubleshooting_install_pydio/troubleshooting_apache_conf_before.png]

So you need to accept all .htaccess files which are in this directory :

[:image-popup:troubleshooting_install_pydio/troubleshooting_apache_conf_after.png]

After that you can restart your apache2:
`sudo service apache2 restart`

And the "Security Breach" error is gone!

###Output buffering issue:

After that you can resolve the last warning, the output buffering, all you need to do it's to edit all of your php.ini:
`output_buffering = Off`

After that you just need to restart apache and the "Pydio Diagnostic Tool".

##MySQL extension issue:

If you see this error when you test your SQL connexion:

[:image-popup:troubleshooting_install_pydio/troubleshooting_mysql_missing.png]

You must install some packages:
`sudo apt-get install php5-mysql`
`sudo service apache2 restart`

You can now test if your database connexion is good.

##Rewrite ( refresh ) issue:

Another problem is when you refresh your browser you have an issue like this:

[:image-popup:troubleshooting_install_pydio/troubleshooting_not_found.png]

If you want to use Pydio features such as Public shares, APIs (sync), WebDav... You must enable rewrite rules.
Maybe you forgot to enable the rewrite mod in apache2:
`sudo a2enmod rewrite`
`sudo service apache2 restart`