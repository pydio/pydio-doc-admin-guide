# Connect Cells Sync dekstop client

## Add a Pydio Cells Account

Before starting to Synchronize data, you must first add your Pydio Cells Account to give access to the **Cells Sync Client**.

- Start your Cells Sync go to **Accounts > Add Account**
- Type the URL of your Pydio Cells, for instance `https://cells.example.com`
- A Browser tab should open inviting you to connect to your Cells
- Type your credentials and press **Enter**
- You will be redirected back to your **Cells Sync Client**.

[:image-popup:1_quick_start/sync-account.gif]

## Create a task to sync a Workspace with a local folder

Once you have your Account set, lets create a task to synchronize **personal-files** to a **local folder**.

- Go to the menu **Tasks > Create Task**.
- In the **Cells Account** section click and choose your preferd Cells Instance.
- Then on the field on its right (path), choose the path.
- In this case we will choose **Personal Files** (blue checkmark on its left, when selected) then click on **Select**.
- In Your **Computer** section select the local path where you want the data to be stored.

[:image-popup:1_quick_start/sync-create-task.gif]


## Notes

- Make sure that the workspaces have **Allow Desktop Sync** enabled.
- If your server is behind a reverse proxy, refer to the reverse proxy documentation.
- For macos might require folder access **System Preferences > Security & Privacy > Files and Folders or Full Disk Access** and allow access.