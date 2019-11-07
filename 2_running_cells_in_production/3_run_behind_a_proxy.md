Proxies are one of the most common way to secure your web servers, it hides the IP of your App server, it can provides TLS encryption for the client/server communication (https) and much more.

For basic usage (Web UX, REST API's, Mobile Applications), a simple HTTP proxy should be fairly easy to set up. However, care must be taken on the Cells `External URL` configuration which will probably differ (your outside world access) from the `Bind URL` configuration (the IP where the Cells server listens to).

If you are using CellsSync, a specific gRPC API is exposed which is relying on HTTP/2 protocol. Your proxy will have to support this protocol (see section below).

### Tutorials

In our knowledge base we have a growing number of comprehensive guides to setup various proxies with Cells:

- [Running Cells behind **Apache2** proxy](/en/docs/kb/deployment/running-cells-behind-apache-reverse-proxy)
- [Reverse proxy with **Caddy**](/en/docs/kb/deployment/running-cells-behind-caddy-reverse-proxy)
- [Reverse proxy with **Nginx**](/en/docs/kb/deployment/running-cells-behind-nginx-reverse-proxy)
- [Docker and **Traefik**](/en/docs/kb/deployment/running-your-cells-docker-behind-traefik-reverse-proxy)
- [Reverse proxy with a Apache your **Docker** instance](/en/docs/kb/deployment/running-your-cells-docker-container-behind-reverse-proxy)

### More details

- See your detailed article in the [Knowledge Base](en/docs/kb/client-applications/setup-cells-server-cellssync).