Here are some common mistakes you might encounter when you try to install Pydio and a way to fix them.

##Permission issue:

After you put your pydio in your /var/www folder you go to your web browser and you may see this:

[:image-popup:1_installation_guide/troubleshooting_forgotten.png]

Pydio need to have some access right to read and/or write files. Just run:
`sudo chown -r www-data:www-data /var/www/pydio/data`

##Diagnostic tool issue:

After refreshing your web browser, you should can see this Pydio Diagnostic Tool:

[:image-popup:1_installation_guide/troubleshooting_diagnostic.png]

###Security breach issue:

So, we have several problems here; first, the "Security Breach" problem is when your data folder is not correctly protected by your apache server. Go to your apache2 folder and edit the "apache2.conf" ( /etc/apache2/apache2.conf ).

You need to edit your conf because from now, your apache2 server ignore all .htaccess files which are in /var/www :

[:image-popup:1_installation_guide/troubleshooting_apache_conf_before.png]

So you need to accept all .htaccess files which are in this directory :

[:image-popup:1_installation_guide/troubleshooting_apache_conf_after.png]

After that you can restart your apache2:
`sudo service apache2 restart`

The "Security Breach" error should now be fixed.

## IoncubeLoader issue:

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

## Output buffering issue:

After that you can resolve the last warning, the output buffering, all you need to do it's to edit all of your php.ini:
`output_buffering = Off`

You can find your php.ini using `php -i | grep "Loaded Configuration File"`.

After that you just need to restart apache and the "Pydio Diagnostic Tool".

## MySQL extension issue:

When you test your SQL connection during the installation wizard, if you have an error "mysql extension missing", make sure to install 
the php-mysql additional packages on your server. For example on a debian: 

`sudo apt-get install php-mysql`  
`sudo service apache2 restart`

You can now test if your database connection is good.

## Rewrite ( refresh ) issue:

Another problem is when you refresh your browser you have an issue like this:

[:image-popup:1_installation_guide/troubleshooting_not_found.png]

If you want to use Pydio features such as Public shares, APIs (sync), WebDav... You must enable rewrite rules.
Maybe you forgot to enable the rewrite mod in apache2:
`sudo a2enmod rewrite`
`sudo service apache2 restart`