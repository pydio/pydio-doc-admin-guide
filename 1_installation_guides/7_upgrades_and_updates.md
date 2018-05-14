### Upgrading from Pydio 8

Upgrading from Pydio 8 and below is **currently not possible** because we have changed most of our code from PHP language to GO language. We are working on a migration process and we will provide you ASAP with a solution. If you are currently using an Enterprise version of Pydio 8 and need to migrate now, please get in touch directly with us.

### Upgrading Cells

For any running Cells / Cells-Enterprise binary, upgrade process is a simple as replacing the old binary with the latest one and restarting the service. You can do that manually by downloading the latest binary, or by using the in-app upgrade via the Settings panel.

Go to **Backend > Upgrade** and then click on check now, after that if there is any update avaiable it will appear. If one or more versions are available, you can select the version you want and the application will automatically upgrade itself, following the steps below: 

* Download the selected binary
* Verify checksum
* Verify crypto signature using the public key embedded in Cells, to make sure this comes from our servers.
* Backup existing binary
* Replace with new binary.

Then you have to manually restart Cells.

### Update Channels

You select the channel in which you want to get the upgrades. By default, use the **stable** channel that will provide you with tested and stable updates. 

The **nightly** channel will provide regular updates but it's up to you as they might be risks involved, so use it with caution.