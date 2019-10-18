The Pydio Cells installer has an automated way of adding a valid SSL certificate making use of the embeded webserver [**Caddy**](https://caddyserver.com) that has an implementation with [**Let's Encrypt**](https://letsencrypt.org/).


## Adding SSL pre Installation


- **Choose SSL Activation mode**:
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.
  - **Use Let's Encrypt**: this option is based on the built-in caddy feature to create and manage certificate using the [Let's Encrypt CA](https://letsencrypt.org/). Beware that if you use this option:
    - your DNS must be correctly configured and the your domain name should point to the correct IP.
    - the user that owns the `cells` process (and thus the underlying caddy server) must have sufficient permission on the `.caddy/` configuration folder.
    _Warning: if you launch the app with an invalid configuration, you might have your DNS temporary black listed on Let's encrypt due to failing multiple retries to get your certificate_.  
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.


## Adding SSL post Installation

# Edit pydio.json --- 1 TODO