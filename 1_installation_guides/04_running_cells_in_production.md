
The best way to run Pydio Cells in a production environment is to start it as a service and put it behind a proxy.
In this page, we gather pointers to various step by step guides from our knowledge base that explain how to achieve this.

### Run Pydio Cells behind a proxy

Proxies are one of the most common way to secure your webservers: it hides the IP of your App server, it can provides TLS encryption for the client/server communication (https) and much more.

In our knowledge base we have a growing number of comprehensive guides to setup various proxies with Cells:

- [proxy with **Apache2**](https://pydio.com/en/docs/kb/devops/proxying-cells-apache)
- [proxy with **Caddy**](https://pydio.com/en/docs/kb/devops/proxying-cells-caddy)
- [proxy with **Nginx**](https://pydio.com/en/docs/kb/devops/proxying-cells-nginx)
- [proxy a **Docker** instance](https://pydio.com/en/docs/kb/devops/proxying-cells-docker)

### Run Cells as a service

Running Cells as a service is a good way to ensure that you have your server running if an event such as a power failure or else happens.
Refer to our knowledge base for a comprehensive guide on how to setup your Cells instance as a service.

- [run cells as a service with **systemd**](https://pydio.com/en/docs/kb/devops/cells-service-systemd)
- [run cells as a service with **supervisor**](https://pydio.com/en/docs/kb/devops/cells-service-supervisor)
