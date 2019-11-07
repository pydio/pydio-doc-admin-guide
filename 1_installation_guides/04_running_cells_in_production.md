<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v1. Looking for <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells v2 docs?</a></span>
</div>

The best way to run Pydio Cells in a production environment is to start it as a service and put it behind a proxy.
In this page, we gather pointers to various step by step guides from our knowledge base that explain how to achieve this.

### Run Pydio Cells behind a proxy

Proxies are one of the most common way to secure your webservers: it hides the IP of your App server, it can provides TLS encryption for the client/server communication (https) and much more.

In our knowledge base we have a growing number of comprehensive guides to setup various proxies with Cells:

- [proxy with **Apache2**](/en/docs/kb/devops/running-cells-behind-apache-reverse-proxy)
- [proxy with **Caddy**](/en/docs/kb/devops/running-cells-behind-caddy-reverse-proxy)
- [proxy with **Nginx**](/en/docs/kb/devops/running-cells-behind-nginx-reverse-proxy)
- [proxy a **Docker** instance](/en/docs/kb/devops/running-your-cells-docker-container-behind-reverse-proxy)

### Run Cells as a service

Running Cells as a service is a good way to ensure that you have your server running if an event such as a power failure or else happens.
Refer to our knowledge base for a comprehensive guide on how to setup your Cells instance as a service.

- [run cells as a service with **systemd**](/en/docs/kb/devops/running-cells-service-systemd)
- [run cells as a service with **supervisor**](/en/docs/kb/devops/running-cells-service-supervisor)
