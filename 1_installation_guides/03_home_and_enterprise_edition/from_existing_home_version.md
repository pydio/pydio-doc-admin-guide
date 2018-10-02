
## Upgrade to Cells Enterprise from an existing Home instance

The upgrade process is easy. Please follow below steps:

1. Download the [Cells Enterprise Binary](https://download.pydio.com/pub/cells-enterprise/release/1.2.0/linux-amd64/cells-enterprise)
1. [Get your Enterprise License key](/en/docs/cells/v1/enterprise-edition-requirements).
1. Put the key in a file named `pydio-license` and put it in `/.config/pydio/cells/`.
1. Stop your running instance and optionnaly remove the `cells` binary.

1. If you are running a linux server and using standard 80 and 443 ports, you must authorize the binary to use these ports:

  ```sh
  setcap 'cap_net_bind_service=+ep' cells-enterprise
```

1. Now run `./cells-enterprise start` to insure the app starts correctly.
1. If you are running under production, please remember you should rather [configure the app to start as a service](https://pydio.com/en/docs/cells/v1/launching-cells-service). In such case, you also have to adapt the supervisor or systemd conf file (depending on your setup) to change the name of the launched binary from `cells` to `cells-enterprise`.