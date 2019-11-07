<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

EasyTransfer is a specific layout that can be applied to any workspace, providing an ultra intuitive interface to "upload-and-share" files in one operation.

## Recommended Setup

While organisations are looking for secure and on-site file sharing solutions, they want to provide simple interface to their users, and eventually have
some users use an even simpler interface without knowing about workspaces, pydio users, etc.

The idea is in that case to let the user login to pydio, but give them access to a single dashboard to quickly drag'n'drop a file and get a public link
in return. In that case, follow the steps below: 

 - Create a "personal" workspace by using the AJXP_USER keyword inside the Path parameter of the workspace driver. That way, under the hood, each user will have her own personal folder for uploading files.
 - Once the workspace is created, use the "Additional Features" to add the "Easy Transfer Layout" feature to this workspace.
 - Create a specific role for denying access to everything else than this workspace to the user : particularly, disable access to the "Home" application page.
 - Grant a read-write access to this workspace on this role.
 - Apply the role to the desired users.
 - If you want to raise the level of security, you can use the parameters list in the role configuration to set "Password Mandatory" on shared links. This will requires the users to enter a password as soon as they upload a file, before creating the actual link.

## Layout

Before dropping a file 

[:image-popup:4_setup_workspaces_and_users/easy_transfer_before.png]

After dropping a file 

[:image-popup:4_setup_workspaces_and_users/easy_transfer_after.png]
