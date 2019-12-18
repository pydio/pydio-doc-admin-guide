Cells embedded web server provides multiple ways for enabling TLS support. This is highly recommended to make sure all communications are encrypted between the client applications and the server (HTTPS).

TLS configuration can be performed at installation time, and you can easily change it later by using the `cells config proxy tls` command.

```sh
$ ./cells config proxy tls
Use the arrow keys to navigate: ↓ ↑ → ←
? Choose TLS activation mode. Please note that you should enable TLS even behind a reverse proxy, as HTTP2 'TLS => Clear' is generally not supported:
  ▸ Provide paths to certificate/key files
    Use Let's Encrypt to automagically generate certificate during installation process
    Generate your own locally trusted certificate (for staging env or if you are behind a reverse proxy)
    Disable TLS (staging environments only, never recommended!)
```

## Supported Modes

### Custom certificate

You can provide a path on the server pointing to your existing Certificate/Key files (PEM format).

**Adding CAs**

If your certificate relies on a third-party Certificate Authority, you have to manually append the CA PEM content to your certificate PEM file.

### Lets Encrypt

Let's Encrypt is an open initiative to make the web safer. It provides free and secure certificates for any domains in an automated way. When switching to this mode, you will have to provide an email address and accept the Let's Encrypt EULA.

**Important notes**

- Your DNS must be correctly configured and the your domain name should point to the correct IP.
- The user that owns the `cells` process (and thus the underlying caddy server) must have sufficient permission on the `$USER_HOME/.caddy/` folder.
- If you launch the app with an invalid configuration, you might have your DNS temporary black listed on Let's encrypt due to failing multiple retries to get your certificate.  

### Generating a Self-Signed certificate

Using this mode, you can easily let Cells generate a cert/key pair and use it to secure connections. This certificate is not recognized by a third-party authority, so it will be displayed as untrusted in a web-browser. 

However, it is a very good solution if you are behind a reverse-proxy to still enable TLS between the proxy and the server. This is in fact recommended if you want to use CellsSync with a Proxy as most proxy will only forward TLS-secured connections to TLS-secured upstream servers.

**About the RootCA**

When enabling self-signed mode, Cells will create (or use) a local RootCA and store it in `${CELLS_WORKING_DIR}/certs`. This CA can possibly be installed on a reverse-proxy machine to validate connection.

Also, for development, if you want your browser to "turn green" (see the certificate as a valid one), you may install this rootCA in your system trust store. Do that easily:

- Install [mkcert](https://github.com/FiloSottile/mkcert) tool
- Export the CAROOT environment variable with value `CAROOT={$CELLS_HOME_USER_DIR}/.config/pydio/cells/certs`
- Run `mkcert -install`
- Relaunch your browser .

### Disabling TLS

You can finally fully disable TLS and let Cells serve connections over HTTP. This is not recommended but is good enough for testing or development. In that case, just beware that the gRPC gateway (required for CellsSync) will be exposed on a separate port that must be opened in the firewall, if any.

## Notes

### Intermediate certificates

If your Certificate Authority(CA) is not recognized by your browser, it might require from you to publish the Intermediate Certificates along with your certificates (Apache2 user might be used to the `SSLCertificateChainFile` directive).

To achieve this on Pydio Cells, you must **concatenate both the certificate and the intermediate certificate** inside the same file. Below is an example of appending the intermediate certificate to your certificate: 

```
cat the_certificate.crt > fullcert.crt
cat intermerdiate_cert.crt >> fullcert.crt
```

You can then proceed to installation, by using the custom certificates option and by providing the new **fullcert.crt** and the **key**.
