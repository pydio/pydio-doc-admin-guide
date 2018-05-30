In this section, we gather known installation problems with strategies to fix them.

## Various Information

### Files location

On a linux-like system, using the `pydio` user, you will find the Pydio files at the following locations:

- **Configuration**: `/home/pydio/.config/pydio/cells/pydio.json`
- **Logs**: `/home/pydio/.config/pydio/cells/logs`.
- **Data**: `/home/pydio/.config/pydio/cells/data`
- **PHP files for the frontend**: `/home/pydio/.config/pydio/cells/static/pydio`

## Connection issues

### PHP-FPM

The most common issue is that you did not start the php-fpm service.

- Verify the status using `sudo service php<version>-fpm status` and then `sudo service php<version>-fpm start` if needed.
- Another common issue is that you did not configured php-fpm to use TCP/UNIX socket.
   - Add `listen = 9000` to the php-fpm.conf file if you're using the TCP socket.
   - Change the `listen.owner=` or `listen.group=` located in `<php-fpm path>/pool.d/www.conf` for the UNIX socket users.


#### Missing Extension

_You installed and started Pydio Cells via command line but land on a blank page when you try to connect to the website_

In the caddy log file, you see such errors:

```sh
30/May/2018:09:31:09 +0200 [ERROR 0 /] PHP message: PHP Fatal error:  Uncaught Error: Class 'DOMDocument' not found in /home/user/go/src/github.com/pydio/cells/assets/src/pydio/core/src/pydio/Core/PluginFramework/Plugin.php:458
Stack trace:
#0 /home/user/dev/go/src/github.com/pydio/cells/assets/src/pydio/core/src/pydio/Core/PluginFramework/PluginsService.php(594): Pydio\Core\PluginFramework\Plugin->loadManifest()
#1 /home/user/dev/go/src/github.com/pydio/cells/assets/src/pydio/core/src/pydio/Core/PluginFramework/PluginsService.php(154): Pydio\Core\PluginFramework\PluginsService->softLoad('core.cache', Array)
#2 /home/user/dev/go/src/github.com/pydio/cells/assets/src/pydio/core/src/pydio/Core/PluginFramework/PluginsService.php(829): Pydio\Core\PluginFramework\PluginsService::cachePluginSoftLoad()
#3 /home/user/dev/go/src/github.com/pydio/cells/assets/src/pydio/core/src/pydio/Core/PluginFramework/PluginsService.php(202): Pydio\Core\PluginFramework\PluginsService->getDetectedPlugins()
#4 /home/user/dev/go/src/github.com/pydio/cell...
``` 

And you might have missed this error message during the install:

```sh
✗ Could not detect PHP version: Detected FPM but there are some missing extensions : dom,curl,intl
``` 

**You miss some php extention**: insure you have all plugins:

```sh
sudo apt install php php-fpm php-gd php-curl php-intl php-xml
```

### Progress during browser install

- No progress appears during install

During the installation process, if there is no progress bar but the console is still showing some activities; it is OK. A manual refresh at the end of the install will load the login page.

[:image-popup:1_installation_guides/troubleshooting_install_no_progress.png]

- The wizard is stucked at the end of the install

After the install, if the page does not refresh automatically in ssl self-signed mode it is OK.  
A manual refresh will load the login page.
