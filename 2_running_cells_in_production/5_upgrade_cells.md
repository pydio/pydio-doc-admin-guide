# TODO



## Update Pydio Cells to the latest version

[:image-popup:2_running_cells_in_production/upgrade_web_ui.png]

```sh
  setcap 'cap_net_bind_service=+ep' cells
```

```sh
  setcap 'cap_net_bind_service=+ep' cells-enterprise
```

## Upgrade Pydio Cells Home to Pydio Cells Enterprise

[:image-popup:2_running_cells_in_production/upgrade_web_ui.png]

Click on **Upgrade to Cells Enterprise**, 
- a menu will appear, proceed by clicking on **start**
- you must accept the terms of the license (check box at the bottom)
- you are now invited to provide your License Key
- press **install now**
- if you are using port 80 or 443 on linux make sure to `setcap` your binary (command below)



```sh
  setcap 'cap_net_bind_service=+ep' cells
```