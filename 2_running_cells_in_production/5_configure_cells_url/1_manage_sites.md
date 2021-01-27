The Cells gateway starts by default on `0.0.0.0:8080`.

By using 0.0.0.0, the gateway will allow incoming traffic to any IPv4/domain of the machine network interfaces.

If the port 8080 is busy (already used by another process), it will try other options.

A self-signed certificate is also automatically generated for supporting TLS connection out of the box (https).

For production use, or for a remotely accessed Cells, you can make the gateway listen to multiple ports on multiple IP addresses,
each with its own set of rules attached (TLS, Let's Encrypt, External URL)

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

EXAMPLES 

Here is a few working samples that you can use for your own setup.

  - Default value (used when no sites is configured)
    - Bind Address: 0.0.0.0:8080
    - TLS : SelfSigned
    - External URL: [left empty]
  
  - Provided certificate
    - Bind Address: 0.0.0.0:8080
    - TLS : Your own key/cert pair
    - External URL: https://share.mydomain.tld
  
  - Auto-ssl using Let's Encrypt 
    - Bind Address: share.mydomain.tld:443
    - TLS : LetsEncrypt
    - External URL: https://share.mydomain.tld
  
  - Self Signed
    - Bind Address: IP:1515         # internal port
    - TLS : SelfSigned
    - External URL: https://IP:8080   # external port mapped by docker
  
  - HTTP only
    - Bind Address: localhost:8080
    - TLS : Disabled
    - External URL: http://localhost:8080  # Non-secured local installation