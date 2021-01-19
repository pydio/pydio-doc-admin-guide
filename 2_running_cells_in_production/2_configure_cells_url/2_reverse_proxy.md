Proxies are one of the most common way to secure your web servers, they can hide the IP of your App server, be the main gateway to your private (virtual) network, serve as TLS Termination point and expose your public certificates, etc.

For basic usage (Web UX, REST API's, Mobile Applications), a simple HTTP proxy is fairly easy to set up. However, you must also add the URL on the authorized list on Cells (which corresponds to EXTERNAL_URL).

To do so run the following command, select the used internal bind and add the proxy URL to it,

```
./cells configure sites
```

Select your used configuration and add your the URL that is going to be used to access through the proxy as the external URL.

_**Warning**: if you intend to use the Cells Sync client together with your server instance, you cannot use TLS Offloading on your reverse proxy. The communication between the sync client and the server is done via gRPC on HTTP/2 and this will not work if you drop from HTTPS to HTTP between your reverse proxy and your Cells instance. Furthermore, note that your proxy has to support this protocol._

### Tutorials

In our knowledge base we have a growing number of comprehensive guides to setup various proxies with Cells:

- [Running Cells behind **Apache2** proxy](/en/docs/kb/deployment/running-cells-behind-apache-reverse-proxy)
- [Reverse proxy with **Caddy**](/en/docs/kb/deployment/running-cells-behind-caddy-reverse-proxy)
- [Reverse proxy with **Nginx**](/en/docs/kb/deployment/running-cells-behind-nginx-reverse-proxy)
- [Docker and **Traefik**](/en/docs/kb/deployment/running-your-cells-docker-behind-traefik-reverse-proxy)
- [Reverse proxy with a Apache your **Docker** instance](/en/docs/kb/deployment/running-your-cells-docker-container-behind-reverse-proxy)

### More details

- See our detailed article in the [Knowledge Base](en/docs/kb/client-applications/setup-cells-server-cellssync).