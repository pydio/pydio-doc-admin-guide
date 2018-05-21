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
