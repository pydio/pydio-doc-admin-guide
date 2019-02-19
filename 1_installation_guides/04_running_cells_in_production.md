## Running cells in production

The best way to run cells in production is to set it as a service and put it behind a proxy, below we will give you some of the most common and most used ways to do that.

### Run cells behind a proxy

Proxies are one of the most common way to secure your webservers for instance you don't expose your servers ip, you can also enable SSL encrypting the communication even more, they can also enhance the performance 

Please refer your our knowledge base for more comprehensive guides on how to setup a proxy with cells.

- [proxy with **apache2**](https://pydio.com/en/docs/kb/devops/proxying-cells-apache)
- [proxy with **caddy**](https://pydio.com/en/docs/kb/devops/proxying-cells-caddy)
- [proxy with **nginx**](https://pydio.com/en/docs/kb/devops/proxying-cells-nginx)
- [proxy a **docker** instance](https://pydio.com/en/docs/kb/devops/proxying-cells-docker)

### Run cells as a service

Running cells as a service is a good way to ensure that you have your server running if an event such as a power failure or else happens.
Refer to our knowledge base for a comprehensive guide on how to setup your cells as a service.

- [run cells as a service with **systemd**](https://pydio.com/en/docs/kb/devops/cells-service-systemd)
- [run cells as a service with **supervisor**](https://pydio.com/en/docs/kb/devops/cells-service-supervisor)
