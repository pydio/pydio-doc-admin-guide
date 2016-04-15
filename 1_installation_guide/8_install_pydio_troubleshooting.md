Here are some common mistake when you try to install Pydio.

All exemples are with the latest [Pydio-enterprise 6.4.1](https://pydio.com/en/community/releases/enterprise-distribution/pydio-enterprise-641-bugfix-release) with the recommend stack ( Linux - Apache2 - MySQL ).

##Permission issue:

After you put your pydio in your /var/www folder you go to your web browser and you see this:

[:image-popup:1_installation_guide/troubleshooting_forgotten.png]

Well, maybe you forgot to grant access to your Pydio for your webserver. Go to your pydio path folder ( /var/www/ ) and type:
`sudo chown -r www-data:www-data /var/www/pydio/data`

##Diagnotic tool issue:

After refreshing, you should see the Pydio Diagnostic Tool:

[:image-popup:1_installation_guide/troubleshooting_diagnostic_tool.png]

###Security breach issue:

So, we have several problems here; first, the "Security Breach" problem is when your data folder is not correctly protected by your apache server. Go to your apache2 folder and edit the "apache2.conf" ( /etc/apache2/apache2.conf ).

Your conf should be edited from that:

[:image-popup:1_installation_guide/troubleshooting_apache_conf_before.png]

To that conf:

[:image-popup:1_installation_guide/troubleshooting_apache_conf_after.png]

After that you can restart your apache:
`sudo service apache2 restart`

And the "Security Breach" error is gone!

##IoncubeLoader issue:

The second and one the most important extension for Pydio-enterprise is "IonCube Extension"; First you need to know if your system it's a 32Bits or 64Bits :
`uname -a`

[:image-popup:1_installation_guide/troubleshooting_uname.png]

As you can see my server is a 64Bits because of the "x86_64", if you have "i386" it's a 32Bits system ( rare now ). After that you need to download the loader ( [32Bits](http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86.tar.gz) [64Bits](http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz) ) and extract it:
`tar xfz ioncube_loaders_lin_x86*`
`cd ioncube`

Then you have several file like this:

[:image-popup:1_installation_guide/troubleshooting_ioncube_ls.png]

The files have a number that corresponds with the PHP version they are made for and there is also a "_ts" (Thread Safe) version of each loader. We will use the version without thread safety here. To find your PHP version tap:
`php -v`

[:image-popup:1_installation_guide/troubleshooting_php_version.png]

You just need to take the first 2 digit ( 5.6 in my case, so i need to take the file "ionucbe_loader_lin_5.6.so" ).Now you have your PHP version and the ioncube file to copy but you also need to have the extension directory of your PHP:
`php -i | grep extension_dir`

[:image-popup:1_installation_guide/troubleshooting_php_extension_dir.png]

You need to take the last directory ( "/usr/lib/php5/20131226" in my case ). Now copy your ioncube file to your PHP extension directory, it will looks like:
`sudo cp ioncube_loader_lin_5.6.so /usr/lib/php5/20131226/`

Now you need to configure your PHP for Ioncube, you just add this part at first line in all your php.ini ( usually in /etc/php5/apache2/php.ini and /etc/php5/cli/php.ini ):
`zend_extension = /usr/lib/php5/20131226/ioncube_loader_lin_5.6.so`
`sudo service apache2 restart`
 To know you can tap:
 `php -v`
 
[:image-popup:1_installation_guide/troubleshooting_php_ioncube.png]

###Output buffering issue:

After that you can resolve the last warning, the output buffering, all you need to do it's to edit all of your php.ini:
`output_buffering = Off`

After that you just need to restart apache and the "Pydio Diagnostic Tool".

##MySQL extension issue:

If you see this error when you test your SQL connexion:

[:image-popup:1_installation_guide/troubleshooting_mysql_missing.png]

You must install some packages:
`sudo apt-get install php5-mysql`
`sudo service apache2 restart`

Restart apache2 and your gone !!

##Rewrite ( refresh ) issue:

Another problem is when you refresh your browser you have an issue like this:

[:image-popup:1_installation_guide/troubleshooting_not_found.png]

If you want to use Pydio features such as Public shares, APIs (sync), WebDav... You must enable rewrite rules.
Maybe you forgot to enable the rewrite mod in apache2:
`sudo a2enmod rewrite`
`sudo service apache2 restart`