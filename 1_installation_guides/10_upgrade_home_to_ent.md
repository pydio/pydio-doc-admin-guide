## How to upgrade your Home Edition to Enterprise Edition

The upgrade process is easy. Please follow below steps:

1. Download the [Cells Enterprise Binary](https://pydio.com/en/download)
2. To retrieve your license key, contact our support.
3. Then put the key in a file named `pydio-license` and put the file in `/.config/pydio/cells/` **(1)**.
4. Stop your the running cells instance and optionnaly remove the `cells` binary.

5. If you are running a linux server and using standard 80 and 443 ports, you must authorize the binary to use these ports:

```sh
  setcap 'cap_net_bind_service=+ep' cells-enterprise
```

1. Now run `./cells-enterprise start` to insure the app starts correctly.
1. If you are running under production, please remember you should rather [configure the app to start as a service](https://pydio.com/en/docs/cells/v1/launching-cells-service). In such case, you also have to adapt the supervisor or systemd conf file (depending on your setup) to change the name of the launched binary from `cells` to `cells-enterprise`.

__(1)__ For macos users the path is `~/Library/Application\ Support/Pydio/cells` the `~` being your home path.