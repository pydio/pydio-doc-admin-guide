You will find gathered all sorts of issues with a way to fix them.

### Troubleshooting

#### Php-fpm

The most common issue is that you did not start the php-fpm service.

* Verify the status using `sudo service php<version>-fpm status` and then `sudo service php<version>-fpm start` if needed.

One common issue is that you did not use and therefore configured php-fpm to use TCP/UNIX socket.

* Add `listen = 9000` to the php-fpm.conf file if you're using the TCP socket.

* Change the `listen.owner=` or `listen.group=` located in ``<php-fpm path>/pool.d/www.conf`` for the UNIX socket users.


#### Progress during  browser install

- No progress appear during install

If the console displays something similar to what is shown in the image below that means there is nothing to worry about. The login page appear after a page refresh.

[:image-popup:1_installation_guide/troubleshooting_install_no_progress.png]