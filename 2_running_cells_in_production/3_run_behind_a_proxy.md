Proxies are one of the most common way to secure your webservers: it hides the IP of your App server, it can provides TLS encryption for the client/server communication (https) and much more.

For basic usage (Web UX, REST API's, Mobile Applications), a simple HTTP proxy should be fairly easy to set up. However, care must be taken on the Cells `External URL` configuration which will probably differ (your outside world access) from the `Bind URL` configuration (the IP where the Cells server listens to).

If you are using CellsSync, a specific gRPC API is exposed which is relying on HTTP/2 protocol. Your proxy will have to support this protocol (see section below).

### Tutorials

In our knowledge base we have a growing number of comprehensive guides to setup various proxies with Cells:

- [Running Cells behind **Apache2** proxy](/en/docs/kb/deployment/running-cells-behind-apache-reverse-proxy)
- [Reverse proxy with **Caddy**](/en/docs/kb/deployment/running-cells-behind-caddy-reverse-proxy)
- [Reverse proxy with **Nginx**](/en/docs/kb/deployment/running-cells-behind-nginx-reverse-proxy)
- [Docker and **Traefik**](/en/docs/kb/deployment/running-your-cells-docker-behind-traefik-reverse-proxy)
- [Reverse proxy with a Apache your **Docker** instance](/en/docs/kb/deployment/running-your-cells-docker-container-behind-reverse-proxy)



### Note: gRPC gateway requirements

For best performances and real-time events, **CellsSync** communicates with the server using a gRPC connection. gRPC is an **HTTP/2** protocol, which implies that **HTTP/2 must be enabled on the bind address facing the outside world**.

If you are behind a proxy or inside a private network, you may have to check your proxy settings: 

- **[TLS Enabled]**  Cert/Key couple, Let's Encrypt, Self-signed config (see note below)
  
  Cells will serve HTTP/1.1 and HTTP/2 on the same port, the one you define for external url (e.g; 443, or 8080 or anything you choose). You don't have to open any other port in your firewall.
  
  - **[No Proxy]** Your Cells is directly facing the outside world with a proper TLS configuration, everything should be working out of the box
  - **[Proxy]** Just make sure your proxy is HTTP/2 enabled. If Cells using self-signed configuration (see note below), you can either install the generated rootCA.pem on the proxy machine, or configure the proxy to SkipVerify (for example *insecure_skip_verify* on Caddy).
  
- **[No TLS]** Cells will serve HTTP/1.1 and HTTP/2 on two different ports. By default, gRPC will pick a randomly available port and advertise it in the /a/config/discovery API. The CellsSync client will automagically query this API to connect. 
  
  - **[No firewall, No Proxy]** If you are on a local machine with all ports open, this should work out of the box.
  - **[Firewall and/or Proxy]** You will have to make proper configuration to open and forward the HTTP/2 on this port. To avoid using a random port at each restart, you can fix this port by using the **PYDIO_GRPC_EXTERNAL** environnement variable at startup. Your proxy will probably not be able to serve HTTPS but HTTP only. 

The various cases are summarized in the figure below.

![api_and_grpc_gateways](https://raw.githubusercontent.com/pydio/cells-dist/master/resources/v2.0.0-rc2/api_and_grpc_gateways.png)

**[Note about self-signed]** : in Cells v1, we were using the embedded self-signed feature of Caddy (our main gateway), that was managed in-memory. Cells v2 has to share the self-signed certificate between the main gateway and the gRPC gateway, so we now generate a self-signed certificate and pass this one to both services. As such, **if you are upgrading from v1 and using self-signed mode**, please re-run the  `cells config proxy tls`  step to regenerate a working configuration.