
In this section, we gather known installation problems with strategies to fix them.

## Various Information

### Files location

On a linux-like system, using the `pydio` user, you will find the Pydio files at the following locations:

- **Configuration**: `/home/pydio/.config/pydio/cells/pydio.json`
- **Logs**: `/home/pydio/.config/pydio/cells/logs`.
- **Data**: `/home/pydio/.config/pydio/cells/data`

## Installation issues

### Progress freezes during browser install

- No progress appears during install

During the installation process, if there is no progress bar but the console is still showing some activities; it is OK. A manual refresh at the end of the install will load the login page.

[:image-popup:1_installation_guides/troubleshooting_install_no_progress.png]

- The wizard is stucked at the end of the install

After the install, if the page does not refresh automatically in ssl self-signed mode it is OK.  
A manual refresh will load the login page.

## Networking issues

### No private address

_ You got this kind of error: `ERROR   nats    Could not run   {“error”: “No private IP address found, and explicit IP not provided”}`_

This typically happens on small VPS that are hosted in the cloud and don't have a private address by default.

The simplest way is to create an alias on the network interface with a 10.0.X address.
For example, if the main interface is eth0, just add the following in the `/etc/network/interface` file.

```conf
auto eth0:1
allow-hotplug eth0:1
iface eth0:1 inet static
address 10.0.0.1
netmask 255.255.255.0
```

Then restart networking services.

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

### Unable to log in

_After a re-install, when trying to login, you get a `could not load session store: securecookie: the value is not valid` error_.

This is bound to the part of the session mechanism that resides in the browser, on client side.

To solve the issue, get rid of all cookie for this site and refresh the page.

## Still stuck?

In case you do not find the answer you are looking for here, please do not ask question in github issues, nor in Twitter or other social feed: our preferred communication channel is our Forum.

So, what should I do in case of:

- Install or upgrade issue? Search the [F.A.Q](https://pydio.com/en/docs/faq) or [READ THE DOCS](https://pydio.com/en/docs)
- No answer yet? [Search our forum](https://forum.pydio.com/)
- Still stuck? It's time to ask the community via the [FORUM](https://forum.pydio.com/)

And only if you're invited to:

- Post a github issue: make sure to put as much detail as possible.
- or submit a pull request.
