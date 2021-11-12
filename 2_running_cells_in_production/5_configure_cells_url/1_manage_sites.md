
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v2. Looking for <a href="https://pydio.com/en/docs/cells/v3/quick-start">Pydio Cells v3 docs?</a></span>
</div>




A site is a configuration item to indicate your network setup to the [Cells Gateway](./glossary). It enforces the ways to access
your Cells and secure them.

The default configuration will get Cells up and running quickly on a server or on a local machine. You can extend to wider network 
and/or secure that access with Sites.

## Command line interface

```
./cells configure sites
```

1. **Bind Address(s)**: one or more `<ip/hostname:port>` to bind Cells to local host's network interfaces addresses. 

2. **TLS Settings**: [TLS configuration](./configure-tls) for HTTPS support (TLS, Let's Encrypt, External URL)

3. **External URL**: Accept traffic coming to this url and redirect to one of the bind address.
    Typically if the app is behind a reverse-proxy or inside a container with ports mapping.

4. **Maintenance Mode [On|Off]**: expose a maintenance page on this endpoint with Cells still running and accessible via the other sites.

## Default Configuration

The Cells gateway starts by default on `0.0.0.0:8080`.

By using 0.0.0.0, the gateway will allow incoming traffic to any IPv4/domain of the machine network interfaces.

If the port 8080 is busy (already used by another process), it will try other options.

A self-signed certificate is automatically generated for supporting TLS connection out of the box.

[:image-popup:2_running_cells_in_production/01-network-local-machine.png]

## Add a Site

All examples below can be used separately or in conjunction with one another.

### Local network example

Let's imagine you want to make your Cells accessible for everybody in your local network via the mypydio.local address. You already have your local DNS
and port fowarding ready so that the incoming traffic redirects to the Cells server on port 8080. You've also generated a certificate, locally trusted.

We indicate all this to Cells using the command and the following parameters:

  - Bind Address: IP:8080
  - TLS: Your own key/cert pair
  - External URL: https://mypydio.local

[:image-popup:2_running_cells_in_production/02-network-local-network.png]

### Public access example

Let's imagine you want to make your Cells accessible via the world wide web. You have a secure reverse proxy running that redirects all traffic to a URL to the
Cells server on port 8081. You can use the self-signed TLS parameter here as the only traffic we need to secure is between the reverse proxy and Cells.

We indicate all this to Cells using the command and the following parameters:

  - Bind Address: IP:8081
  - TLS: Self-Signed
  - External URL: https://mypydio.com

[:image-popup:2_running_cells_in_production/03-network-public-access.png]

## Advanced Usage [ED]

Configure some Cells functionalities to behave differently depending on the site accessed.

### [Authentication Connectors](./ed-using-sso-external-identity-provider)

Display different ways of authenticating on different URLs.

### [Security Policies](./ed-security-policies)

Add the URL in the context of the security policies to for example:

  - restrict access to the intranet site to office hours only.
  - allow access to the www site to specifc IPs.