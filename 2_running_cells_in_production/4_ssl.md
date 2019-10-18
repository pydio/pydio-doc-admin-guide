# TODO

- **Choose SSL Activation mode**:
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.
  - **Use Let's Encrypt**: this option is based on the built-in caddy feature to create and manage certificate using the [Let's Encrypt CA](https://letsencrypt.org/). Beware that if you use this option:
    - your DNS must be correctly configured and the your domain name should point to the correct IP.
    - the user that owns the `cells` process (and thus the underlying caddy server) must have sufficient permission on the `.caddy/` configuration folder.
    _Warning: if you launch the app with an invalid configuration, you might have your DNS temporary black listed on Let's encrypt due to failing multiple retries to get your certificate_.  
  - **Generate a self-signed certificate**: Pydio Cells generates a self signed certificate. Be advised this should **not** be used for system in production.
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.