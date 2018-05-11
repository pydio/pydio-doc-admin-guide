You will find gathered all sorts of issues with a way to fix them.

### Troubleshooting

#### Php-fpm

The most common issue is that you did not start the php-fpm service.

* Verify the status using `sudo service php<version>-fpm status` and then `sudo service php<version>-fpm start` if needed.

One common issue is that you did not use and therefore configured php-fpm to use TCP/UNIX socket.

* Add `listen = 9000` to the php-fpm.conf file if you're using the TCP socket.

* Change the `listen.owner=` or `listen.group=` located in ``<php-fpm path>/pool.d/www.conf`` for the UNIX socket users.
