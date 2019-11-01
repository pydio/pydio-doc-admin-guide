# Connect Cells Sync dekstop client

## Download Cells

## TODO update links for final release

- [**Windows**](https://download.pydio.com/pub/cells-sync/release/0.9.0/windows-amd64/)
- [**MacOS**](https://download.pydio.com/pub/cells-sync/release/0.9.0/darwin-amd64/)
- [**Linux**](https://download.pydio.com/pub/cells-sync/release/0.9.0/linux-amd64/)

> Linux distributions might require additional packages.

## Install

- **Windows** you have an **msi** installer that will guide you.
- **MacOS** drag and drop the **CellsSync.app** to your **applications** folder.
- **Linux**

## Add a Pydio Cells Account

In order to be able to synchronize data, you must configure the connection from your Cells Sync Client to your Cells server with a valid account.

- Start your Cells Sync go to **Accounts > Add Account**,
- Type the URL of your Pydio Cells, for instance `https://cells.example.com`,
- A new tab in your default browser (as defined in your OS) opens and invites you to connect to your Cells server,
- Type your credentials and press **Enter**,
- You are then redirected back to your **Cells Sync Client**.

[:image-popup:1_quick_start/sync-account.gif]

## Create a task to sync a Workspace with a local folder

Now that your account is set, let's create a task to synchronize your **personal-files** space on your server to a **local folder** of your choice.

- Go to the menu **Tasks > Create Task**.
- In the **Cells Account** section click and choose the correct connection.
- Then choose a path on the `path` field on its right.
- In our current example, we choose **Personal Files** (blue checkmark on its left, when selected) and then click on **Select**.
- In  **Your Computer** section, select the local path where you want the data to be stored.

[:image-popup:1_quick_start/sync-create-task.gif]

## Notes

- Make sure that the corresponding workspace in your Cells server has **Allow Desktop Sync** enabled.
- If your server is behind a reverse proxy, refer to the reverse proxy documentation.
- MacOS users might require additional permissions; **System Preferences > Security & Privacy > Files and Folders** and allow access.
- Linux Desktop users might require an additional package to display the webview.

## Use cases

## TODO add links to knowledge base

There is a lot of applicable use cases that will be explain in our knowledge base:

- Synchronize - Datasource <-> Datasource
- Synchronize - Datasource <-> S3 storage
- Synchronize - Datasource (my-cells.com) <-> Datasource (my-other-cells.com)
