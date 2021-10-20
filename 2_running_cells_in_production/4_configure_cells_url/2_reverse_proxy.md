Reverse proxies are one of the most common way to secure web servers. You can make them hide the IP of your App server, serve as the main gateway to your private (virtual) network, or as TLS Termination point and expose your public certificates, etc.

A simple HTTPS reverse-proxy is fairly easy to set up for basic usage (Web UX, REST API's, Mobile Applications). You just need to make sure that the URL is known to Cells so that it can allow incoming traffic from this address.

Run the following command, 

```
./cells configure sites
```

Select an active site or add a new [site](./manage-sites) and set the reverse-proxy URL in the [External URL](./glossary) field.

_**Warning**: if you intend to use CellsSync client together with your server instance, you cannot use TLS Offloading on your reverse proxy. The communication between the sync client and the server is done via gRPC on HTTP/2 and this will not work if you drop from HTTPS to HTTP between your reverse proxy and your Cells instance. Furthermore, note that your proxy has to support this protocol._

### Tutorials

In our knowledge base we have a growing number of comprehensive guides to setup various proxies with Cells:

- [Running Cells behind **Apache2** proxy](/en/docs/kb/deployment/running-cells-behind-apache-reverse-proxy)
- [Reverse proxy with **Caddy**](/en/docs/kb/deployment/running-cells-behind-caddy-reverse-proxy)
- [Reverse proxy with **Nginx**](/en/docs/kb/deployment/running-cells-behind-nginx-reverse-proxy)
- [Docker and **Traefik**](/en/docs/kb/deployment/running-your-cells-docker-behind-traefik-reverse-proxy)
- [Reverse proxy with a Apache your **Docker** instance](/en/docs/kb/deployment/running-your-cells-docker-container-behind-reverse-proxy)

### More details

- See our detailed article in the [Knowledge Base](en/docs/kb/client-applications/setup-cells-server-cellssync).