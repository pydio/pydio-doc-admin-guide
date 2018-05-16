### Upgrading from Pydio 8

Upgrading from Pydio 8 and below is **currently not possible** because we have changed most of our code from PHP language to GO language. We are working on a migration process and we will provide you ASAP with a solution. If you are currently using an Enterprise version of Pydio 8 and need to migrate now, please get in touch directly with us.

### Upgrading Cells

On a running Cells / Cells-Enterprise system, upgrade process is as simple as replacing the old binary with the latest one and restarting the service. You can do it manually (by downloading the latest binary) or use the in-app upgrade via the `Settings` panel.

To perform the in-app upgrade: 

- Go to **Backend > Upgrade** 
- Click on `Check now`
- If there is any update avaiable it appears.
- If more than one version is available, select the version you want
- The application automatically upgrades, following these steps: 
   - Download the selected binary
   - Verify checksum
   - Verify crypto signature using the public key embedded in Cells, to make sure this comes from our servers.
   - Backup existing binary
   - Replace with new binary.
- You then have to manually restart Pydio Cells

### Update Channels

You can select the channel in which you want to get the upgrades. By default, use the **stable** channel that only publishes carefully tested and non breaking updates. 

The **nightly** channel publishes updates more often with the latest version of the code but with higher risks of failure or systems errors. Use it with caution and only on non-production facig systems.