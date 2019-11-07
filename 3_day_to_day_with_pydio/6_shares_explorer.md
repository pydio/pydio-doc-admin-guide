<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 8 (PHP), EOL end 2019. Time to move to <a href="https://pydio.com/en/docs/cells/v2/quick-start">Pydio Cells</a>!</span>
</div>

Pydio 8 brings a new tool for administrator to more easily browse every files and folders shared on the platform. This is useful to spot mistakes and anomalies, 
or simply generate reports based on users or workspaces.

## Shares Explorer

The shares explorer allows you to browse the shares by grouping them successively by the following parameters : 
 
 - Users
 - Workspaces
 - Share Type (public link or workspace)
 
You can select for each level one of this parameter to trigger the next column. 
 
[:image-popup:3_day_to_day_with_pydio/shares_explorer_first_select.png]
 
For each grouping, it shows you a number of shares: beware that this number may sometimes be approximate. As this listing does not check the consistency of each and every
 share it counts, some underlying broken links may be counted but will not appear in the final listing. 
 
[:image-popup:3_day_to_day_with_pydio/shares_explorer_full_tree.png]

## Editing Shares

Once you have a full list of shares, you cannot open a share, that will appear in the next right panel. You cannot change all parameters of this share, but one of the three actions are available: 

- **Open corresponding file** This will work only if you have a proper access to the containing workspace
- **Delete Share** This will stop this file or folder from being shared.
- **Close** Simply close the panel to open another share.

[:image-popup:3_day_to_day_with_pydio/shares_explorer_open_share.png]