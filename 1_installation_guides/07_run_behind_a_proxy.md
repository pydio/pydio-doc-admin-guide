_In this section, we introduce some examples configuration to run Pydio Cells behind a proxy._

## Using Apache as a reverse proxy

### Specific Pydio Cells Configuration

During the installation process, instead of the offered settings you should enter configuration similar to this:

```sh
Binding Host (Internal, Other): cells.example.com:7070
External Host: cells.example.com
```

* Binding Host : address where the application http server is bound to. It MUST contain a server name and a port.
* External host : url the end user will use to connect to the application.
* Example:
  If you want your application to run on the localhost at port 8080 and use the url mycells.mypydio.com, then set CELLS_BIND to localhost:8080 and CELLS_EXTERNAL to mycells.mypydio.com

**If you wish to use the 0.0.0.0 address you must respect this rule, cells_bind has to be exactly like this `cells_bind=0.0.0.0:<port>` and `cells_external=<domain name,address>:<port>`, the *port* is mandatory in both otherwise you will have a grey screen stuck in the loading**

### Configure Apache

You must enable the following mods with apache :
- `proxy`
- `proxy_http`
- `proxy_wstunnel`

Edit Apache mod_ssl configuration file to have this:

```conf
Listen 8080
<VirtualHost *:8080>
ServerName demo.fr
ServerAdmin demo.fr
  AllowEncodedSlashes On
  RewriteEngine On
  #SSLProxyEngine On
  #SSLProxyVerify None
  #SSLProxyCheckPeerCN Off
  #SSLProxyCheckPeerName Off

  #Proxy WebSocket
  #RewriteCond %{HTTP:Upgrade} =websocket [NC]
  #RewriteRule /(.*) wss://127.0.0.1:8080/$1 [P,L]
  ProxyPassMatch "/ws/(.*)" ws://192.168.0.172:8080/ws/$1 nocanon

  #Finally simple proxy instruction
  ProxyPass "/" "http://192.168.0.172:8080/"
  ProxyPassReverse "/" "http://192.168.0.172:8080/"

  #Uncoment if you are going to use SSL
  #SSLEngine on
  #SSLCertificateFile /etc/ssl/localcerts/server.crt
  #SSLCertificateKeyFile /etc/ssl/localcerts/server.key
  #SSLCertificateChainFile /etc/ssl/localcerts/bundled.crt

ErrorLog ${APACHE_LOG_DIR}/error-ssl.log
CustomLog ${APACHE_LOG_DIR}/access-ssl.log combined
</VirtualHost>
```
> For this example my proxy is running on `192.168.0.176`, while my cells is running on another server `192.168.0.172:8080` .

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

If you Caddy reverse proxy is not on the same machine as your Cells instance you may have to take a look at the following clues.


To properly configure the certificates that you want to use, please refer to the [tls plugin page of the caddy documentation](https://caddyserver.com/docs/tls).

## Run your Docker Container behind an Apache reverse Proxy using SSL

The process is pretty much the same as the previous example on apache,
when you run you docker container either with the command `docker run` or in a docker-compose file have to specify `CELLS_BIND` and `CELLS_EXTERNAL`.

Here's what CELLS_BIND and CELLS_EXTERNAL mean to give a you a general understanding.

```
CELLS_BIND : address where the application http server is bound to. It MUST contain a server name and a port.
CELLS_EXTERNAL : url the end user will use to connect to the application.
Example:
If you want your application to run on the localhost at port 8080 and use the url mycells.mypydio.com, then set CELLS_BIND to localhost:8080 and CELLS_EXTERNAL to mycells.mypydio.com
```
**If you wish to use the 0.0.0.0 address you must respect this rule, cells_bind has to be exactly like this `cells_bind=0.0.0.0:<port>` and `cells_external=<domain name,address>:<port>`, the *port* is mandatory in both otherwise you will have a grey screen stuck in the loading**

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
