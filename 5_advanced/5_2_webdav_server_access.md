In this section, we see how to use the webdav protocol alongside Pydio Cells various client interfaces to access your files.

### Webdav with Pydio Cells

Webdav is integrated out of the box. You can access it with valid credentials at the following address: 
 `<the-address-of-your-pydio>/dav/`.

For instance:

_I have a Pydio Cells installed on a server that i access from `192.168.0.1:8080`._: therefore to access the webdav, i would have to use the following address `http(s)://192.168.0.1:8080/dav/`.  

With domain names, it goes the same: if your pydio's domain name is `example.com` then use `http(s)://example.com/dav/`.

_For the moment webdav is incompatible with self-signed certificates._

What webdav offers is a list and complete access to every workspace and cells that a user has rights to, meaning that you can browse them using different clients (refer to the link below for a how-to use a webdav client with Pydio Cells).

_Be advised that webdav is not a sync client. If you are off-line and therefore have no access to the server, you will not be able to interact with your files. Proceed with caution._

### Compatible webdav clients

This is a list of the compatible clients that have been tested.

[Guides to webdav clients](https://pydio.com/en/docs/kb/miscellaneous/use-webdav-clients)

* MacOS finder
* Windows explorer
* Ubuntu (and other linux distributions)
* Cyberduck client (windows and macos client)