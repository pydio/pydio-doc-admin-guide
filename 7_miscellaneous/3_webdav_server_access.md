
Webdav protocol is integrated out of the box in Pydio Cells.

You can access your server with a webdav client and valid credentials at the following address:   
`<the-address-of-your-pydio>/dav/`.

For instance:

_I have a Pydio Cells installed on a server that I can access at `http://192.168.0.1:8080`_: therefore to access the webdav, I would have to use the following address `http://192.168.0.1:8080/dav/`.  

With domain names, it goes the same: if your pydio's domain name is `example.com` then use `http(s)://example.com/dav/`.

### Implemented features

- List and browse of workspaces and cells depending on current user rights and permissions.
- Copy, move, rename, delete of files and folders.
- Upload and download.

*Warnings*:

- For the time being, webdav is incompatible with self-signed certificates.
- Webdav is **not** a sync client. If you are off-line and therefore have no access to your server, you will not be able to interact with your files. Proceed with caution.
- If you are running Cells behind a reverse proxy, you must forward webdav related headers along with the forwarded requests (using the _transparent_ directive usually does the trick).

### Compatible webdav clients

Following clients have been tested and are known to work with a recent Pydio Cells (v1.5+):

- MacOS finder
- Windows explorer
- Ubuntu (and other Linux distributions)
- Cyberduck client (Windows and MacOS client)

You can find a short setup guide for each one of them in our [knowledge base](https://pydio.com/en/docs/kb/miscellaneous/use-webdav-clients).
