In this chapter, we explain how to upgrade from Pydio Cells Home Edition to Pydio Cells Enterprise Edition.

### Upgrade to Pydio Cells Enterprise from an existing Pydio Cells Home instance

The upgrade process is not that hard but it requires you to follow every step to the letter :

1. Download the [Cells Enterprise Binary](https://download.pydio.com/pub/cells-enterprise/release/0.9.1/linux-amd64/cells-enterprise)
1. Replace your `cells` binary with the `cells-enterprise`binary.
1. [Get your Enterprise License key](/en/docs/cells/v1/enterprise-edition-requirements).
1. Put the key in a file named `pydio-license` and put it in `/.config/pydio/cells/`.
1. Clear pydio's cells cache. You might use this command (with care):
    
    ```sh
    rm -f ~/.config/pydio/cells/static/pydio/data/cache/plugins_*
    ```

1. Now run `./cells-enterprise start` and you should be good to go.