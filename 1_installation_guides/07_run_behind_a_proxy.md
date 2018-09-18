_In this section, we introduce some examples configuration to run Pydio Cells behind a proxy._ 

For the time being, we only cover Apache.

## Using Apache as a reverse proxy

### Specific Pydio Cells Configuration

During the installation process, instead of the offered settings you should enter configuration similar to this:

```sh
Binding Host (Internal, Other): cells.example.com:7070
External Host: cells.example.com
```

### Configure Apache

Edit Apache mod_ssl configuration file to have this:

```conf
<IfModule mod_ssl.c>
<VirtualHost *:443>
  ServerName domain.pydio.com
  # May be necessary for API direct accesses
  AllowEncodedSlashes On
  RewriteEngine On
   # Make sure to proxy SSL
  SSLProxyEngine On
  # Disable SSLProxyCheck : maybe necessary if Cells is configured with self_signed
  SSLProxyCheckPeerCN Off
  SSLProxyCheckPeerName Off
  SSLProxyVerify none

  # Proxy WebSocket
  RewriteCond %{HTTP:Upgrade} =websocket [NC]
  RewriteRule /(.*)           wss://cells.example.com:7070/$1 [P,L]
   # Finally simple proxy instruction
  ProxyPass "/" "https://cells.example.com:7070/"
  ProxyPassReverse "/" "https://cells.example.com:7070/"
</VirtualHost>
</IfModule>
```

Please note:

- The AllowEncodedSlashes On that may be necessary if not activated globally in apache (to call APIs like /a/meta/bulk/path%2F%to%2Ffolder
- When I configure Cells, even on another port, I actually **make sure to bind it directly to the cells.example.com** as well (like Apache). This is necessary for the presigned URL used with S3 API for uploads and downloads (they used signed headers and a mismatch between received Host headers may break the signature). Another option is to still bind Cells using a local IP, then in the Admin Settings, under Configs Backend, use the field “Replace Host Header for S3 Signature” and use the internal IP here.

## Using [Caddy](https://caddyserver.com) as a reverse proxy

The example below shows the configuration of a proxy that serves the demo.pydio.com url and redirects to a Pydio Cells environment running on port 8080 of the localhost :

```conf

demo.pydio.com {
  log stdout

  tls /etc/certs/pydio.crt /etc/certs/pydio.key

  timeouts 0

  # And the rest to pydio
  proxy / localhost:8080 {
    insecure_skip_verify
    transparent
    websocket
  }
}
```

To properly configure the certificates that you want to use, please refer to the [tls plugin page of the caddy documentation](https://caddyserver.com/docs/tls).


## Run your Docker Container behind an Apache reverse Proxy using SSL

The process is pretty much the same as the previous example on apache,
when you run you docker container either with the command `docker run` or in a docker-compose file have to specify `CELLS_BIND` and `CELLS_EXTERNAL`.

To illustrate the concept above an example is provided.

For instance you have your cells container running on a server that has an address such as : `192.168.1.12`
you decide to run your container on port `7070` and therefore to run your container behind the proxy you will to have set
`CELLS_BIND = 192.168.1.12:7070` and `CELLS_EXTERNAL = 192.168.1.12` (can be in the docker-compose or as environement variables in the docker run command).

If you want to use SSL do not forget to also put `CELLS_NO_SSL = 0` that is SSL on cells side but even if you want to use SSL for your Apache Proxy you will have to enable it (and set the certificates path for the proxy to use).


Then create configuration file for apache proxy (if used as it is , it will work when you have ssl enabled on both the proxy and cells) with the following:

```conf
<IfModule mod_ssl.c>
<VirtualHost *:443>
  ServerName domain.pydio.com
  # May be necessary for API direct accesses
  AllowEncodedSlashes On
  RewriteEngine On
   # Make sure to proxy SSL
  SSLProxyEngine On
  # Disable SSLProxyCheck : maybe necessary if Cells is configured with self_signed
  SSLProxyCheckPeerCN Off
  SSLProxyCheckPeerName Off
  SSLProxyVerify none

  # The Certificate path
    SSLCertificateFile /home/user/cert/apache.crt
    SSLCertificateKeyFile /home/user/cert/apache.key

  # Proxy WebSocket
  RewriteCond %{HTTP:Upgrade} =websocket [NC]
  RewriteRule /(.*)           wss://192.168.0.153:8080/$1 [P,L]
   # Finally simple proxy instruction
  ProxyPass "/" "https://192.168.1.12:7070/"
  ProxyPassReverse "/" "https://192.168.1.12:7070/"

</VirtualHost>
</IfModule>
```
