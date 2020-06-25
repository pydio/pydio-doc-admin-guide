Cells binary is self-contained and can be easily updated / upgraded using either the in-app tool, the command-line, or by simply downloading a new binary and replacing the existing one. All necessary migrations will be performed on version-change detection. Downgrading is not possible though, as downward migrations are generally not implemented.

## In-App Tool

### Update Pydio Cells to the latest version

[:image:2_running_cells_in_production/update_cells_webui.png]

- You can check if there are new updates available by pressing the **Check** button,
- if that is the case select the version and proceed to install once it's finished you will be invited to restart your Cells instance.
- on Linux **make sure** if you are bound to ports (80 or 443) to set the capabilities with :

```sh
  setcap 'cap_net_bind_service=+ep' cells
```

### Upgrade Pydio Cells Home to Pydio Cells Enterprise

[:image:2_running_cells_in_production/upgrade_cells_to_ent_webui.png]

Click on **Upgrade to Cells Enterprise**:

- A menu will appear, proceed by clicking on **start**.
- You must accept the terms of the license (check box at the bottom).
- You are now invited to provide your License Key.
- Press **install now**.
- Make sure if you are using port 80 or 443 on linux to `setcap` your binary (command below)

```sh
  setcap 'cap_net_bind_service=+ep' cells
```

## Command Line

### Update Pydio Cells to the latest version

- Run the command `./cells update`.
- Identify the latest version number available (for instance 1.6.2).
- To update run `./cells update --version=1.6.2`.
- Make sure if you are under linux and are using port 80/443 to set the capabilities with `setcap 'cap_net_bind_service=+ep' cells`.
- Restart Cells.

### Upgrade Pydio Cells Home to Pydio Cells Enterprise

- Stop your Cells
- Download the latest Cells-enterprise binary and replace replace the current one (if you are using systemd either rename the binary to cells or update your cells.service target)
- Add the license (logged as the user running Cells), create this file `~/.config/pydio/cells/pydio-license` and put your License Key.
- Restart Cells.

## Notes

### Security

To provide an additional security layer and to avoid MITM attack, all binaries downloaded from the official update server are signed with our private key. Before applying upgrade, your Cells server will always check the validity of the package it just downloaded.

### Do not forget setcap!

After Updating always make sure to set the capabilities if you are running on a linux server.

```sh
  setcap 'cap_net_bind_service=+ep' cells
```

### Upgrading from Cells to Cells Enterprise

After upgrading to Enterprise, make sure that you have the license file, located in `~/.config/pydio/cells/pydio-license`.
