In this chapter we will see how you can use the webdav protocol along side pydio to access your files from other means.

### Configure and use webdav Access

With Pydio Cells webdav is now by default integrated, you can now access it through `<the-address-of- your-pydio>/dav/` and then you will be asked to log in using your pydio login and password, we will give you examples to give you a clear picture of the principle.

For instance i have pydio installed on a server and i access it through `192.168.0.1:8080`, then to access the webdav i will have to use the following `http(s)://192.168.0.1:8080/dav/`,
the same goes for domain names my pydio's domain name is for instance `my_pydio.net` then use `http(s)://my_pydio.net/dav/`.

*Be advised that webdav is not a sync client, therefore if you don't have access to the server you will not be able to interact with it proceed with caution.*

### Using webdav with the integrated file browsers of linux, mac and windows machines.

* **For MacOS** : do the following **go to your finder > right click > connect to a server** and then you will have a menu displaying, add your address like the examples above `http://192.168.0.1:8080/dav/` after that you will be prompted to put in your login informations, you can now access to your files located in your pydio server through the macos finder, it is mounted as a remote disk so if you the connection is lost you also loose your ability to see/interact with the finder.

* **For ubuntu(linux users)** : for ubuntu users or other linux users you can use nautilus the file manager, and as seen in this screenshot (ubuntu 18) if you click on other locations, you will have a field on the bottom allowing you to put the address of the webdav for instance `http://192.168.0.1:8080/dav/` after that you'll be prompted for login and password and you're good to go.

* **Windows** :  For windows 10 users go to **This pc** on top you will have a menu **Add a network connection** then put the informations of your webdav connection, when you are finishied with that on the top menu go to **Map a network drive** then choose a letter and the drive which will be the network one that you added previously and it's done you can now browse and use it as you wish.
