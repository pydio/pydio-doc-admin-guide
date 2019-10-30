The Pydio Cells installer has an automated way of adding a valid SSL certificate making use of the embeded webserver [**Caddy**](https://caddyserver.com) that has an implementation with [**Let's Encrypt**](https://letsencrypt.org/).

## Adding SSL pre Installation

- **Choose SSL Activation mode**:
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.
  - **Use Let's Encrypt**: this option is based on the built-in caddy feature to create and manage certificate using the [Let's Encrypt CA](https://letsencrypt.org/). Beware that if you use this option:
    - your DNS must be correctly configured and the your domain name should point to the correct IP.
    - the user that owns the `cells` process (and thus the underlying caddy server) must have sufficient permission on the `.caddy/` configuration folder.
    _Warning: if you launch the app with an invalid configuration, you might have your DNS temporary black listed on Let's encrypt due to failing multiple retries to get your certificate_.  
  - **Generate a self signed certificate**: will generate a self signed certificate using the [mkcert](https://github.com/FiloSottile/mkcert) tool, the certificate is stored under `~/.config/pydio/cells/certs`.

## Adding SSL post Installation

## Change certificate with the  Cells binary

[:image-popup:2_running_cells_in_production/cells_cli_ssl_mod.png]

You can change at any time your with the Cells binary, by running the following command:
- Make sure that your Cells is stopped (meaning service too)
- `./cells config ssl mode`

Once it's done restart your cells Instance.
  