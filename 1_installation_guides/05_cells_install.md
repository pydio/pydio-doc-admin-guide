
_Once you have prepared your environment following one of the OS specific guide of the [previous section](/en/docs/cells/v1/os-specific-guides), the installation process of Pydio Cells is self explanatory and straight forward. You can yet find more details and explanations in this section_.

If you are installing the enterprise edition, you should first [get your license key](/en/docs/cells/v1/enterprise-edition-requirements) to be able to complete the installation.

## Launching the installer

First, give execution rights to the binary. For instance: `sudo chmod u+x <binary>`.

Then, to launch the installer:

- [optional but recommended] switch to / log in as the specific user you have created to run Pydio Cells
- go to the directory where your binary resides
- simply type:

```sh
# Home edition
./cells install
# Enterprise edition
./cells-enterprise install
```

A first **Installation mode**  menu appears in your terminal, choose if you want to use a browser or go on in the terminal to install Pydio Cells.

Then, define the url parameters of your instance:

- **Internal Url**: internal url for Pydio Cells: the various services reaches each other through the gateway at this address. In most common cases, it is `<ip>:<port>`(example: `192.168.0.192:80`). If you are going to use SSL you should put your secure port `443` or else.
- **Choose SSL Activation mode**:
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.
  - **Use Let's Encrypt**: this option is based on the built-in caddy feature to create and manage certificate using the [Let's Encrypt CA](https://letsencrypt.org/). Beware that if you use this option:
    - your DNS must be correctly configured and the your domain name should point to the correct IP.
    - the user that owns the `cells` process (and thus the underlying caddy server) must have sufficient permission on the `.caddy/` configuration folder.
    _Warning: if you launch the app with an invalid configuration, you might have your DNS temporary black listed on Let's encrypt due to failing multiple retries to get your certificate_.  
  - **Generate a self-signed certificate**: Pydio Cells generates a self signed certificate. Be advised this should **not** be used for system in production.
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.
- **External Url**: world facing URL of Pydio Cells. Usually, you can use the same URL as the bind URL. Thus this field is auto-filled with the value you have just entered and can be left untouched. In the case where you want to set up a more exotic configuration, adapt the URL here the external Url must always have the protocol such as for instance `https://my-domain.com` (or http).
The external Url is your mean to access Cells therefore if you are running a reverse proxy this will in the mean case be like this , external_url = reverse_proxy_url used to access.
 
_Subsequent steps are then pretty much the same in the browser or in the CLI_.

## Installation process

1. [ED] **Enterprise License key**: put the license key here. Please refer to the [Enterprise Edition Requirements guide] (en/docs/cells/v1/enterprise-edition-requirements) to get one if necessary.
1. **Database connection**: database connection parameters. DB user must have `ALL PRIVILEGES` granted on the corresponding database.
1. **Admin User**: Cells' admin user informations.
1. **Advanced Settings**: you might change here the ports that are used internally by the various services when communication over the bind URL. Default used ports are non invasive, so you probably can skip this part.
1. **Apply Installation**: you are done. A progress bar appears while the parameters are applied.
    - if you are using the web-based installer, the page reloads automatically when ready. Don't quit this page or press anything.
    - if you are using the CLI installer, you can stop and restart the app once you have seen the success message.

## Next steps

To stop Pydio Cells, press `CTRL + C` in the terminal where you launched the installer.

You might start it again using this command to ensure everything work as expected:

```sh
# Home edition
./cells start
# Enterprise edition
./cells-enterprise start
```

Yet this is not the recommanded way to start Pydio Cells when running in production. Please refer to the [following chapter](/en/docs/cells/v1/running-cells-in-production) to finalize integration of your Cells instance with the host system.

If you encounter any issue during the installation process, please refer to the [generic troubleshooting section of this guide](/en/docs/cells/v1/troubleshooting) or to the specific trouble shooting section that is at the bottom of each one of the [OS specific installation guides](/en/docs/cells/v1/os-specific-guides).

In case you do not find any answer there, you should also have a look [at our forum](https://forum.pydio.com/) where our friendly community will be happy to help.