[Cells Gateway](./glossary) provides multiple ways for enabling TLS support. It is highly recommended to make sure all communications are encrypted between the client applications and the server (HTTPS).

TLS configuration can be changed by using the `cells configure sites` command.

```sh
./cells configure sites
```

## Supported Modes

### Custom certificate

You can provide a path on the server pointing to your existing Certificate/Key files (PEM format).

**Adding CAs**

If your certificate relies on a third-party Certificate Authority, you have to manually append the CA PEM content to your certificate PEM file.

### Lets Encrypt

Let's Encrypt is an open initiative to make the web safer. It provides free and secure certificates for any domains in an automated way. When switching to this mode, you will have to provide an email address and accept the Let's Encrypt EULA.

**Important warning:**

If you launch the app with an invalid configuration or if your domain name is not correctly registered and propagated by your DNS, the certicate generation will fail, possibly more than once, due to multiple retries and errors.
You might then quickly reach Let's encrypt rate limits: this can lead to having your FQDN black listed, at least temporarily.

Thus, **we strongly advise to first use the staging entry point** when configuring your network setup, by positively answering this question:

```
✔ Do you want to use Let's Encrypt staging entrypoint? [y/N] : y
```

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
cat intermediate_cert.crt >> fullcert.crt
```

You can then proceed to installation, by using the custom certificates option and by providing the new **fullcert.crt** and the **key**.