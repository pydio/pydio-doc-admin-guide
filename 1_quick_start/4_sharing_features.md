Pydio Cells delivers file sharing and collaboration in a way that is familiar, comfortable and intuitive for users of modern collaborative apps. It lets users decide how to share files and information, based on knowledge of their own teams, workflows and working patterns. This end-user freedom also takes the responsibility of creating effective workspaces away from overburdened administrators.

## Sharing with Public links

You can share files or folders using a public link. To create a public link, choose the folder/file that you want to share and right click on it: a menu appears. Click on **Share**, it opens a configuration popup:

[:image:1_quick_start/sharing_features/public_link_0.png]

Click on **Enable Public Links** to generate a unique URL providing public access to this resource.

[:image:1_quick_start/sharing_features/public_link_1.png]

**Note**: The generated link URL is computed from your current web access. If you are using multiple URLs to access to Cells, you can force Cells to use only one of them for all public links. Go to Cells Console > Main Settings panel and also refer to [Configure Cells URL](./configure-cells-urls) in this documentation.

Once a link is enabled, you can fine-tuned and restrict its accesses with various configurations described below

### Permissions

[:image:1_quick_start/sharing_features/public_link_permissions.png]

Set up basic read/write/preview permissions for this link. Options here may depend on the shared resource type (file or folder) and the additional restrictions set. 

- **For a folder**, you can pick from "preview" (show files previews), "read" (open/download files) and "upload" (let people upload additional contents to the folder. Worth noting, you can share a folder with only the "upload" option set, providing a blind dropping box to collect files from external users.
- **For a file**, you can pick from "preview" (show preview), "read" (open/download file) or "edit" if the file type provides a supported editor inside Pydio Cells interface.

### Labels and Display Options

[:image:1_quick_start/sharing_features/public_link_labels.png]

- **Link Label**: the label of your link; you have a menu displaying all your public links. It is easier to recognize them with a label)
- **Link Description**: filled by default with the creation date, this is displayed in the various public layouts seen by the end-user that clicks on the link. You can change this to provide a more adequate description of the content.
- **Layout**: customize data presentation with specific templates. For example, if you are sharing a folder filled with images, use the Gallery template to directly display images as a mosaic. 

### Access restrictions

[:image:1_quick_start/sharing_features/public_link_access.png]

This section provides additional restrictions capabilities for accessing this link : 

- **Password** will prompt user for a password before accessing the link
- **Expiration Date** will automatically disable the link after a certain period of time
- **Maximum Downloads** (only for files with "download" permission) will automatically disable the link after the file has been downloaded a given number of times.

### Advanced Visibility

[:image:1_quick_start/sharing_features/public_link_advanced.png]

This advanced feature is less used but can be handy: by default, a link created on a file or folder is "owned" by the current user. Other users who access the same workspace cannot see this link, nor edit its characteristics. This settings allows you to "hand over" the link management to another user, or for example to let a whole group of people use this link and edit its properties. 

Beware of not confusing the link permissions/accesses (what can a public user see at this link) and the visilibity (which internal user is allowed to edit this link properties). This "visibility" concept can be found elsewhere in the application, regarding shared users managements, Cells management, etc.
 
## Sharing with other users using Cells

The Cells concept will be familiar to modern workers used to collaborating on _"channels"_ within popular group-chat applications. Cells can be shared internally or externally to the organization, with users able to add new individuals and groups to the Cell. In-app, instant messaging within each Cell, provides a focused channel for group communication around the theme.

End-users can create their own, flexible Cells: combining files and folders from any location, or picking users from their address book.

### Creating Cells from folders/files

When using the **Share** action (either in right-click menu or in the information column), one can directly add a folder or a file to an existing cell, or create a new cell with the current folder.

[:image:1_quick_start/sharing_features/create_cell_4.png]

### Creating Cells from scratch

When using the "Create Cell" button from the left-hand panel, the following menu is displayed:

[:image:1_quick_start/sharing_features/create_cell_1.png]

- **Title**: the name for the cell.
- **Description**: add a short description of your cell to inform users about the topic.

[:image:1_quick_start/sharing_features/create_cell_2.png]

- **Choose who you will share data with**: Pick a single or multiple groups/users that will have access to this cell. You can also choose how they can interact within it.

[:image:1_quick_start/sharing_features/create_cell_3.png]

- **Add an existing Folder**: Pick an existing folder from any of your current workspaces, to feed the Cell with data. You can also leave the cell empty and a dedicated folder will be created on the underlying storage.

Click on **Create Cell Now!** to finish the process.

### Creating Cells from Address Book

When browsing the Address Book in the right-hand column, clicking on the vertical dots for a user opens, providing following options:

- **Create a cell with this user**
- **Add user to current Cell**: if you are currently inside a Cell
- **Remove from current Cell**: if the user is part of the current Cell and you have the right to edit this Cell.
