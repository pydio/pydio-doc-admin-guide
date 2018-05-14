In this chapter we will see how you can upgrade from Pydio Cells Home Edition to Pydio cells Enterprise Edition.

### Upgrade to Pydio Cells Enterprise from an existing Pydio Cells Home instance

The upgrade process is not that hard but it requires you to follow every step to the letter :

1. Download the [Cells Enterprise Binary](https://download.pydio.com/pub/cells-enterprise/release/0.9.1/linux-amd64/cells-enterprise)

2. Replace your `cells` binary with the `cells-enterprise`binary.

3. [Get your Enterprise License key](link to license key guide).

4. Put the key in a file named `pydio-license` and put it in `/.config/pydio/cells/`.

5. Clear pydio's cells cache using for instance `rm -rf ~/.config/pydio/cells/static/pydio/data/cache/plugins_*`

6. Now run `cells-enterprise start`and you should be good to go.