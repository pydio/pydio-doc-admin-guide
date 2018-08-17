In this section, we see how to use the webdav protocol alongside Pydio Cells various client interfaces to access your files.

### Configure and use webdav Access

Webdav is integrated out of the box. You can access it with valid credentials at the following address: `<the-address-of- your-pydio>/dav/`.

For instance:

_I have Pydio Cells installed on a server. I access it at `192.168.0.1:8080`._: to access the webdav, use the following address `http(s)://192.168.0.1:8080/dav/`.  

With domain names, it goes the same: if your pydio's domain name is `example.com` then use `http(s)://example.com/dav/`.

*Be advised that webdav is not a sync client. If you are off-line and therefore have no access to the server, you will not be able to interact with your files. Proceed with caution.*

### Using webdav with integrated file browsers

#### MacOS

Do the following: **go to your finder > right click > connect to a server**.

In the menu that pops up, add your address like in the examples above, for instance: `http://192.168.0.1:8080/dav/`

You are then prompted to put in your login informations.

You can now access to your files located in your pydio server through the macos finder. It is mounted as a remote disk: if you the connection is lost you also loose your ability to see/interact with the finder.

#### Ubuntu (Linux users)

Ubuntu or other linux users can use the Nautilus file manager.

As seen in the below screenshot (ubuntu 18), if you click on `Other locations`, you open a field on the bottom allowing you to enter the address of the webdav.  
For instance: `http://192.168.0.1:8080/dav/`. You are then prompted for login and password and you are good to go.

#### Windows

Windows 10 users must:

- Go to **This pc** 
- On the top part, click on **Add a network connection**
- Put the information of your webdav connection

Once you have done this:

- go in the top menu to **Map a network drive**
- choose a letter and the network drive that you previously added

It's done. You can now browse and use it as you wish.
