
_Once you have prepared your environment following one of the OS specific guide of the [previous section](/en/docs/cells/v1/os-specific-guides), the installation process of Pydio Cells is self explanatory and straight forward. You can yet find more details and explanations in this section._

If you are installing the enterprise edition, you should first get your license key to be able to complete the installation. Please [refer to this guide](/en/docs/cells/v1/enterprise-edition-requirements) to do so.

## Launching the installer

First, give execution rights to the binary. For instance: `sudo chmod u+x <binary>`.

Then, to launch the installer:

- [optionnal but recommanded] switch to / log in as the specific user you have created to run Pydio Cells
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

- **Bind Host**: internal url for Pydio Cells: the various services reaches each other through the gateway at this address. In most common cases, it is `<ip>:<port>`(example: `192.168.0.192:80`). If you are going to use SSL you should put your secure port `443` or else.
- **External HOST**: world facing URL of Pydio Cells. Usually, you can use the same URL as the bind URL. Thus this field is auto-filled with the value you have just entered and can be left untouched. In the case where you want to set up a more exotic configuration, adapt the URL here.
- **Choose SSL Activation mode**:
  - **Provide paths to certificate/key files**: if you already have a certificate/key, you can use them.
  - **Generate a self-signed certificate**: Pydio Cells generates a self signed certificate. Be advised this should **not** be used for system in production.

_Subsequent steps are then pretty much the same in the browser or in the CLI_.

## Installation process

1. [ED] **Enterprise License key**: put the license key here. Please refer to the [Enterprise Edition Requirements] (en/docs/cells/v1/enterprise-edition-requirements) guide if you have not yet your key.
1. **Database connection**: database connecion parameters. The user that is used must have all permission granted on the corresponding database.
1. **PHP-FPM Detection**: The installer detects your version of php-fpm and if there are missing packages.
   If you have a warning because you're using php 5.5.9, don't worry and press next.
1. **Admin User** : Pydio's Admin user informations.
1. **Advanced Settings** : you might change here the ports that are used internally by the various services when communication over the bind URL. Default used ports are non invasive, so you probably can skip this part.
1. **Apply Installation** : you are done. A progress bar appears while the parameters are applied.
    - If you are using the web-based installer, the page reloads automatically when ready: don't quit this page or press anything.
    - If you are using the CLI installer, you can stop and restart the app once you have seen the success message.

## Next steps

To stop Pydio Cells, press `CTRL + C` in the terminal where you launched the installer.

You might start it again using this command to insure everything work as expected:

```sh
# Home edition
./cells start
# Enterprise edition
./cells-enterprise start
```

Yet this is not the recommanded way to start Pydio Cells when running in production. Please refer to the [following chapter](/en/docs/cells/v1/launching-cells-service) to finalise integration of your Pydio Cell with the host system.

If you ever encounter issue during the installation process, please refer to the generic [trouble shooting section](/en/docs/cells/v1/troubleshooting) of this chapter or to the specific trouble shooting section that is at the bottom of each one of the [OS specific installation guides](/en/docs/cells/v1/os-specific-guides).