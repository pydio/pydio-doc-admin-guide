A site is a configuration item that indicates to the Cells Gateway how you have setup your network, making it accessible from multiple contexts.

The default configuration will get Cells up and running quickly on a server or on a local machine. You can extend that access
to wider area network with Sites.

## Command line interface

```
./cells configure sites
```

1. **Bind Address(s)**: one or more `<ip/hostname:port>` to bind Cells to local host's network interfaces addresses. 

2. **TLS Settings**: TLS configuration for HTTPS support (TLS, Let's Encrypt, External URL)

3. **External URL**: Accept traffic coming from this url and redirect to one of the bind address.
    Typically if the app is behind a reverse-proxy or inside a container with ports mapping.

4. **Maintenance Mode [On|Off]**: expose a maintenance page on this endpoint, although Cells is running.

## Default Configuration

The Cells gateway starts by default on `0.0.0.0:8080`.

By using 0.0.0.0, the gateway will allow incoming traffic to any IPv4/domain of the machine network interfaces.

If the port 8080 is busy (already used by another process), it will try other options.

A self-signed certificate is automatically generated for supporting TLS connection out of the box.

[:image-popup:2_running_cells_in_production/01-network-local-machine.png]

## Add a site

All examples can be used separately or in conjunction with one another.

### Local network example

Let's imagine you want to make your Cells accessible for everybody in your local network via the mypydio.local address. You already have your local DNS
and port fowarding ready so that the incoming traffic redirects to the Cells server on port 8080. You've also generated a certificate, locally trusted.

We indicate all this to Cells using the command with the following parameters:
  - Bind Address: IP:8080
  - TLS: Your own key/cert pair
  - External URL: https://mypydio.local

[:image-popup:2_running_cells_in_production/02-network-local-network.png]

### Public access example

Let's imagine you want to make your Cells accessible via the world wide web. You have a secure reverse proxy running that redirects all traffic to a URL to the
Cells server on port 8081. You can use the self-signed TLS parameter here as the only traffic we need to secure is between the reverse proxy and Cells.

We indicate all this to Cells using the command with the following parameters:
  - Bind Address: IP:8081
  - TLS: Self-Signed
  - External URL: https://mypydio.com

[:image-popup:2_running_cells_in_production/03-network-www.png]

## Advanced Usage [ED]

Configure some Cells functionalities to behave differently depending on the context they are accessed from.

### Authentication Connectors [TODO - link]
Display different ways of authenticating on different URLs.

### Security Policies [TODO - link]
Add the URL in the context of the security policies to for example : 
  - restrict access to the intranet to office hours exclusively.
  - allow access to the www to specifc IPs.


# [TODO - REMOVE]
For production use, or for a remotely accessed Cells, you can make the gateway listen to multiple ports on multiple IP addresses,
each with its own set of rules attached 

- At installation time using specific flags (`--bind`, `tls_xxx`): see the [man page of the configure command TODO](LINK TO DOC).
- With the dedicated tool `./cells configure sites`: see this [dedicated page for more information about the "sites" (TODO)](LINK TO PAGE).

Manage how Cells is binding to the local host's network interfaces addresses and accepting traffic coming from external URLs.
  This is the main tool for listing, editing, adding and removing sites. Additional sub-commands allow you to directly create/delete sites.
  
  Each site has following parameters:
  
1. **Bind Address(s)**: one or more `<ip/hostname:port>` to bind Cells to local host's network interfaces addresses. 

2. **TLS Settings**: TLS configuration for HTTPS support.

3. **External URL**: Accept traffic coming from this url and redirect to one of the bind address.
    Typically if the app is behind a reverse-proxy or inside a container with ports mapping.

4. **Maintenance Mode [On|Off]**: expose a maintenance page on this endpoint, although Cells is running.