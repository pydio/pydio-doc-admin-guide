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

If during the install there is no progress and the console is show some activities it is OK. A manual refresh at the end of the install will laod the login page.

[:image-popup:1_installation_guides/troubleshooting_install_no_progress.png]


- The wizard is stucked at the end of the install

If after the install the page does not refresh automatically in ssl self-signed mode it is OK. A manual refresh will laod the login page