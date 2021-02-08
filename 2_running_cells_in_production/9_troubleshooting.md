You can find below a list of problems commonly encountered when installing and configuring Pydio Cells Server or Sync Client, together with strategies to diagnose and fix them.

You can find more use cases and solution in [our FAQ](https://pydio.com/en/docs/faq), [our Knowledge Base](https://pydio.com/en/docs/knowledge-base) and [our forum](https://forum.pydio.com/).

## Cells Server

### Unable to bind port 443

_You have configured the [Binding Address](./glossary) with port 443 and enabled redirection of port 80. You get an error page "Unable to connect" when you try to connect_.

Check the error log, if you see such an error:

```sh
2018-05-21T10:55:57.532+0200   ERROR   pydio.grpc.gateway.proxy   Could not run   {"error": "listen tcp :443: bind: permission denied"}
```

You probably did not give permission to the `cells` binary file to use reserved ports. To fix this:

```sh
sudo setcap 'cap_net_bind_service=+ep' /path/to/your/cells/binary
```

### 0.0.0.0 address

If you are behind a reverse proxy and get `404: this page is not served on this interface` when trying to access the web UI, you can try to use the `0.0.0.0` generic IP address in your [Binding Address](./glossary).

This basically tells to the [Cells Gateway](./glossary) of the application to accept all requests on this port.

In contrary, if you give a specific valid IP or a domain name in the Binding Address, the Cells Gateway checks the "HOST" header of the incoming requests. If the header does not **exactly** matches with the DN or IP of the Binding Address, the server throw a 404 file not found error.

Remember, when the Binding Address is `0.0.0.0:<port>` (the port is compulsory), and if you are not using a well-known port 80 or 443, you must also include the port in the external URL that must look like:  `<scheme>://<IP or DN>:<port>`, for instance `http://localhost:8080`, otherwise, you might stay stuck on a grey _loading_ page.

### Hydra Error after install

On first start or just after the installation terminated, you get such an error:

```sh
 [0002] Could not ensure that signing keys for "hydra.openid.id-token" exists. This can happen if you forget to run "hydra migrate sql", set the wrong "secrets.system" or forget to set "secrets.system" entirely.  error="cipher: message authentication failed"
```

It usually means you have performed a new install on top of a former install without correctly cleaning (a.k.a dropping) the DB.

### Unable to log in

_After a re-install, when trying to login, you get a `could not load session store: securecookie: the value is not valid` error_.

This is bound to the part of the session mechanism that resides in the browser, on client side.
To solve the issue, get rid of all cookie for this site and hard refresh the page.

### I locked my user

You can unlock the admin or any user with the CLI (assuming that you have access on the server), run the following command `./cells admin user unlock -u <username>`.

### I forgot the password of a user

Once again with the CLI (assuming that you have access on the server), you can run `./cells admin user set-pwd -u <username> -p <new password>`

### Database server issue

_After start, the web page is unreachable and you see a bunch of errors starting with: `ERROR   pydio.grpc.meta   Failed to init DB provider   {"error": "Error 1071: Specified key was too long; max key length is 767 bytes handling data_meta_0.1.sql"}`_.

You might have an unsupported version of the mysql server: you should use MySQL server version 5.7 or higher or MariaDB version 10.3 or higher.

### Various

_You see this error: `/lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.14' not found`_

The version of libc6 is outdated. Run these commands to upgrade it.

```sh
sudo apt-get update
sudo apt-get install libc6
```

## Cells Sync

### Cannot list workspaces

If you are getting an error when you attempt to list the workspaces then your server might be running behind a reverse proxy, in this case you must use **TLS** on **Cells** and on the **reverse proxy**, please refer to our dedicated documentation [Running Cells Behind a reverse Proxy](./configure-cells-reverse-proxy).

### I do see not my workspaces

Make sure that your workspaces can be synchronized (enable settings in workspace menu)

### Unable to create or list folder on local system

- MacOSX users might require permissions, **System Preferences > Security & Privacy > Files and Folders** and allow access.

### Missing package (LINUX)

- If you are getting this error (on Gnome):

```sh
./cells-sync: error while loading shared libraries: libappindicator3.so.1: cannot open shared object file: No such file or directory
```

- Then install `sudo apt install libappindicator3-1`

## Still stuck?

In case you do not find the answer you are looking for here, please do not ask question in github issues, nor in Twitter or other social feed: our preferred communication channel is our Forum.

So, what should I do in case of:

- Install or upgrade issue? Search the [F.A.Q](https://pydio.com/en/docs/faq) or [READ THE DOCS](https://pydio.com/en/docs)
- No answer yet? [Search our forum](https://forum.pydio.com/)
- Still stuck? It is time to ask the community via the [FORUM](https://forum.pydio.com/)

And only if you are invited to:

- Post a github issue: make sure to put as much detail as possible.
- or submit a pull request.
