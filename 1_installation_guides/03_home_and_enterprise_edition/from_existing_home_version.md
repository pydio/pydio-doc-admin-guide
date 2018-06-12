In this chapter, we explain how to upgrade from Pydio Cells Home Edition to Pydio Cells Enterprise Edition.

### Upgrade to Pydio Cells Enterprise from an existing Pydio Cells Home instance

The upgrade process is not that hard but it requires you to follow every step to the letter :

1. Download the [Cells Enterprise Binary](https://download.pydio.com/pub/cells-enterprise/release/1.0.0/linux-amd64/cells-enterprise)
1. [Get your Enterprise License key](/en/docs/cells/v1/enterprise-edition-requirements).
1. Put the key in a file named `pydio-license` and put it in `/.config/pydio/cells/`.
1. Stop your running instance and optionnaly remove the `cells` binary.

1. Clear pydio's cells cache. You might use this command (with care):
    
    ```sh
    rm -f ~/.config/pydio/cells/static/pydio/data/cache/plugins_*
    ```
1. If you are running a linux server and using standard 80 and 443 ports, you must authorize the binary to use these ports:
    
    ```sh
    setcap 'cap_net_bind_service=+ep' cells-enterprise
    ```

1. Now run `./cells-enterprise start` to insure the app starts correctly.
1. If you are running under production, please remember you should rather [configure the app to start as a service](https://pydio.com/en/docs/cells/v1/launching-cells-service).