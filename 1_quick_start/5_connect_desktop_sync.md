## Disclaimers

Providing CellsSync to your users can be an efficient way to help them keep their files offline on their desktop computer. 

However, please be aware of the following:  

 - CellsSync is still in testing phase. We are getting a lot of positive feedbacks but it may not yet be 100% bullet-proof! Make sure to always backup your data on a regular basis.
 - Whatever the sync client, having many users connected on the same folder can eventually lead to conflicts, that users may have to solve manually. We recommend allowing users to sync only their Personal workspace as a starting point.
 
## Server Pre-requisites

Please beware that many users syncing high amount of files can severy load the server, your installation must be prepared for that! Also, if you are behind a reverse proxy, you may check the dedicated section to learn more about gRPC handling through reverse proxies.

For Cells Enterprise, Sync is enabled by administrator on a per-workspace basis. By default, only `Personal` workspace is sync-enabled. If you wish to enable other workspaces, you have to switch the `Allow Desktop Sync` flag in each of them.

## Installing CellsSync

### Windows

Download the [**CellsSync MSI (TODO)**](https://download.pydio.com/pub/cells-sync/release/0.9.0/windows-amd64/) and execute it on the computer. It will install CellsSync in the application, at the moment it is not launched automatically.

### MacOSX

Download the [**CellsSync DMG (TODO)]() and execute it. Drag and drop CellsSync in the Applications folder, then launch it. The very first launch will alert about the file coming from the internet, in some cases you may have to launch it again after approving this alert.

Depending on your security config, MacOS might require additional permissions; **System Preferences > Security & Privacy > Files and Folders** and allow access.

### Linux

There is currently no installer for Linux, just download the [**CellsSync Binary (TODO)**]() and run it with `./cells-sync start` command.

Depending on distributions, the UX components might require additional packages (TODO).

## Configuring CellsSync

In order to be able to synchronize data, you must configure the connection from your CellsSync Client to your Cells server with a valid account. Let's create a task to synchronize your **personal-files** space on your server to a **local folder** of your choice.

- Go to the menu **Tasks > Create Task**.
- Choose `new server` : type the URL of your Pydio Cells, for instance `https://cells.example.com`
- A new tab in your default browser (as defined in your OS) will open and invite you to connect to your Cells server
- Once logged it, you will be redirected back to your CellsSync Client. The server must now be available in the **Cells Account** selector. 
- Select the server then choose a path on the `path` field on its right.
- In our current example, we choose **Personal Files** and then click on **Select**.
- In **Your Computer** section, select the local path where you want the data to be stored.
- Click Save : the task should be created and start syncing.

[:image:1_quick_start/sync-create-task.gif]