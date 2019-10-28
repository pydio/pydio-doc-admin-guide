# TODO


In this section, we gather known installation problems with strategies to fix them.

## Cells Server

### Files location

On a linux-like system, using the `pydio` user, you will find the Pydio files at the following locations:

- **Configuration**: `/home/pydio/.config/pydio/cells/pydio.json`
- **Logs**: `/home/pydio/.config/pydio/cells/logs`.
- **Data**: `/home/pydio/.config/pydio/cells/data`

### Progress freezes during browser install

- No progress appears during install

During the installation process, if there is no progress bar but the console is still showing some activities; it is OK. A manual refresh at the end of the install will load the login page.

[:image-popup:1_quick_start/troubleshooting_install_no_progress.png]

- The wizard is stucked at the end of the install

After the install, if the page does not refresh automatically in ssl self-signed mode it is OK.  
A manual refresh will load the login page.

### Unable to bind port 443

_You have configured the bind URL with port 443 and enable redirection of port 80. You get an error page "Unable to connect" when you try to connect_.

Check the error log, if you sse such an error:

```sh
2018-05-21T10:55:57.532+0200   ERROR   pydio.grpc.gateway.proxy   Could not run   {"error": "listen tcp :443: bind: permission denied"}
```

You probably did not give permission to the `cells` binary file to use reserved ports. To fix this:

```sh
sudo setcap 'cap_net_bind_service=+ep' cells
```

### 0.0.0.0 address

If you wish to use the 0.0.0.0 address you must respect this rule, cells_bind has to be exactly like this `cells_bind=0.0.0.0:<port>` and `cells_external=<domain name,address>:<port>`, the *port* is mandatory in both otherwise you will have a grey screen stuck in the loading

### Unable to log in

_After a re-install, when trying to login, you get a `could not load session store: securecookie: the value is not valid` error_.

This is bound to the part of the session mechanism that resides in the browser, on client side.

To solve the issue, get rid of all cookie for this site and refresh the page.


## Cells Sync


### Cannot list workspaces

You might have a reverse proxy in this case you must strictly use https on cells and on the reverse proxy, please refer to our dedicated documentation [link to reverse proxy how-to]().

### I do not my workspaces

Make sure that your workspaces are syncable (enable settings in workspace menu)

### Unable to create or list folder on local system

- Macos users might require permissions, **System Preferences > Security & Privacy > Files and Folders** and allow access.

## Still stuck?

In case you do not find the answer you are looking for here, please do not ask question in github issues, nor in Twitter or other social feed: our preferred communication channel is our Forum.

So, what should I do in case of:

- Install or upgrade issue? Search the [F.A.Q](https://pydio.com/en/docs/faq) or [READ THE DOCS](https://pydio.com/en/docs)
- No answer yet? [Search our forum](https://forum.pydio.com/)
- Still stuck? It's time to ask the community via the [FORUM](https://forum.pydio.com/)

And only if you're invited to:

- Post a github issue: make sure to put as much detail as possible.
- or submit a pull request.
