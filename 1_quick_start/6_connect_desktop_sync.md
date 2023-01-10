Providing CellsSync to your users can be an efficient way to help them keep their files offline on their desktop computer.  However, please be aware of the following:  

- When the sync client has many users connected to the same folder, some conflicts might arise. The users may then have to solve the conflicts manually. We recommend allowing users to sync only their `Personal` workspace as a starting point.
- CellsSync is still in testing phase. We are getting a lot of positive feedback but it may not yet be 100% bullet-proof! Make sure to always backup your data on a regular basis.

## Server Pre-requisites

Please be aware that many users syncing high amount of files can severely load the server, your installation must be prepared for that! Also, if you are behind a reverse proxy, you may check the dedicated section to learn more about gRPC handling through reverse proxies.

For Cells Enterprise, Sync is enabled by an administrator on a per-workspace basis. By default, only `Personal` workspace is sync-enabled. If you wish to enable other workspaces, you have to switch the `Allow Desktop Sync` flag in each of them.

## Installing CellsSync

### Windows

Download the [CellsSync MSI](https://download.pydio.com/latest/cells-sync/release/{latest}/windows-amd64/CellsSync-{latest}.msi) and execute it on your computer. It installs CellsSync as a windows application. For the time being, the local service is not automatically restarted at reboot.

### MacOSX

Download the [CellsSync DMG](https://download.pydio.com/latest/cells-sync/release/{latest}/darwin-amd64/CellsSync-{latest}.dmg) and execute it. Drag and drop CellsSync in the Applications folder, then launch it. The very first launch will alert about the file coming from the internet, in some cases you may have to launch it again after approving this alert.

Depending on your security config, MacOS might require additional permissions; **System Preferences > Security & Privacy > Files and Folders** and allow access.

### Linux

There is currently no installer for Linux, just download the [CellsSync Binary](https://download.pydio.com/latest/cells-sync/release/{latest}/linux-amd64/cells-sync) and run it with `./cells-sync start` command.

Depending on distributions, the UX components might require additional packages, such as `sudo apt install libappindicator3-1`.

## Configuring CellsSync

In order to be able to synchronize data, you must configure the connection from your CellsSync Client to your Cells server with a valid account. Let's create a task to synchronize your **personal-files** space on your server to a **local folder** of your choice.

- Go to the menu **Tasks > Create Task**.
- Choose `new server` : type the URL of your Pydio Cells, for instance `https://cells.example.com`
- A new tab in your default browser (as defined in your OS) opens and invites you to provide your credentials
- Once logged, you are redirected back to your CellsSync Client. The server is now available in the **Cells Account** selector.
- Select the server and choose a path on the `path` field on its right.
- In our current example, we choose **Personal Files** and then click on **Select**.
- In **Your Computer** section, select the local path where you want the data to be stored.
- Click Save: the task is created and starts synchronizing you data.

[:image:1_quick_start/cells_sync_basic.gif]

## Server compatibility : Cells v4 vs. older versions

Starting with v4, a new hashing mechanism has been introduced to compute files signature in a storage-independant way. This signature or "hash" is used for fast comparison between two files version: if they are the same, the file did not change. 

### Datasource Hashing Version

Cells v3 and before were previously relying on the storage eTag feature, and the Sync client would on its side compute a hash in the same format (md5). We now use an internal hashing mechanism, which brings consistency accross all storages, all upload methods (direct PUT or multipart), and datasource encryption status. 

If you are upgrading from Cells v3 to v4, file hashes in all datasources **will not be automatically recomputed**, as it is a I/O and CPU intensive operation: you may want to schedule it when it suits you best. 

To trigger this operation, please use the following command ([more info here](../developer-guide/cells-admin-datasource-rehash)) 

```
$ cells admin datasource rehash -u admin --datasource {YOUR_DATASOURCE_NAME}
```

This will:
 
 - List all files in datasource and recompute their hash
 - Update datasource configuration with a specific "hashing_version=v4" attribute
 - If datasource is structured, update the "resync" job to always rehash new files after synchronization. 

Once the job is finished (see in the Scheduler or Cells Flows), you can restart the server.

### Compatibility Matrix

On your desktop, if you update from 0.9.2 or below, CellsSync will automatically recompute all hashes on the local filesystem and trigger a full resynchronization. From there, the matrix below shows how a given datasource will be compatible with a given version of CellsSync.

| Cells Server + Datasource Status | Cells Sync <= 0.9.2 | Cells Sync >= 0.9.3 | 
|----------------------------------|---------------------|---------------------|
| Cells v3 or older                | OK                  | FAIL                |
| v3 upgraded to v4 w/o rehash     | OK (needs re-auth)  | FAIL                |
| v3 upgraded to v4 w/ rehash      | FAIL                | OK                  |
| Cells v4 new installation        | FAIL                | OK                  |

**So if you do not plan to update your server to v4, do not upgrade Cells Sync after 0.9.2** (you can reinstall older version if you updated by mistake)!

## Troubleshooting

If you encounter errors such as:

- `/oidc/.well-known 404` this error might indicate that the `EXTERNAL_URL` on Cells was not set, please refer to the following [article](./configure-cells-reverse-proxy) on how to configure this paremeter.
- `transport is closing` or `server closed the stream without sending trailers`: This might be related to the reverse-proxy **grpc** configuration. [see knowledge base](https://pydio.com/en/docs/kb/deployment)