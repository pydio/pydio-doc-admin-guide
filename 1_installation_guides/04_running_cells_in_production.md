
The best way to run Pydio Cells in a production environment is to start it as a service and put it behind a proxy. We gather in this page relevant resources that points toward various step by step guides that explain how to achieve this.

### Run Pydio Cells behind a proxy

Proxies are one of the most common way to secure your webservers: you do not expose your server's ip, you can enable SSL cipher communication with clients and even more.

Please refer your our knowledge base for comprehensive guides on how to setup a proxy with Cells.

- [proxy with **Apache2**](https://pydio.com/en/docs/kb/devops/proxying-cells-apache)
- [proxy with **Caddy**](https://pydio.com/en/docs/kb/devops/proxying-cells-caddy)
- [proxy with **Nginx**](https://pydio.com/en/docs/kb/devops/proxying-cells-nginx)
- [proxy a **Docker** instance](https://pydio.com/en/docs/kb/devops/proxying-cells-docker)

### Run Cells as a service

Running Cells as a service is a good way to ensure that you have your server running if an event such as a power failure or else happens.
Refer to our knowledge base for a comprehensive guide on how to setup your Cells instance as a service.

- [run cells as a service with **systemd**](https://pydio.com/en/docs/kb/devops/cells-service-systemd)
- [run cells as a service with **supervisor**](https://pydio.com/en/docs/kb/devops/cells-service-supervisor)
