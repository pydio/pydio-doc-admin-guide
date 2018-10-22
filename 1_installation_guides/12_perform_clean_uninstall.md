## Clean uninstallation

In this guide we will go through the steps to perform a clean uninstallation.

### How to perform a clean uninstall

On most of the OS pydio cells does not put the resources in multiple folders, it is concentrated in a single folder.
To remove cells you only have to remove the pydio cells folder and remove the cells database.

To remove the folder on _linux_ distributions you can make use of rm such as :
`rm -r ~/.config/pydio/cells`.

To remove the cells folder on _MacOS_ this :
`rm -r ~/Library/Application\ Support/Pydio/cells`.
_For macos users do not make the mistake to remove the 'Pydio' folder the sync app also stores configuration inside it_.

For any additional datasource that you are not going to make use of, you can just clear the parent folder.

Then clean the database with this command after you have logged in mysql :
`drop database cells;` (or any other name if you named it).

And you are good to go, there is no more traces of the previous install or of cells in general.
