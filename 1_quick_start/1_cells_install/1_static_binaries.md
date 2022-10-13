
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>



_These instructions will get you started quickly with Cells binaries. Apply commands similary on either `cells` of `cells-enterprise` binary._

<ol class="install-steps">
<li><p><strong>Get your database access information</strong> (see <a href="./requirements">Requirements</a>). Login as non-root user.</p></li>
<li><p><a href="/en/download" target="_blank"><strong>Download the binary</strong></a> for your server architecture. Make it executable:<br> <code>$ chmod +x ./cells</code></p></li>
<li><p><strong>Configure Cells</strong> using web or command line installer (see below):<br> <code>$ ./cells configure</code></p></li>
<li><p><strong>Start Cells</strong>. Web installer restarts automatically, otherwise use: <code>$ ./cells start</code></p></li>
<li><p><strong>Open your web browser</strong> at <a href="https://localhost:8080" target="_blank">https://localhost:8080</a> <br> (or https://[server ip or domain]:8080).</p></li>
</ol>

<style type="text/css">
ol.install-steps {
  padding-left: 0 !important;
  list-style: none;
  counter-reset: my-awesome-counter;
  padding: 0;
  margin:0;
}
ol.install-steps li {
  counter-increment: my-awesome-counter;
  border-left: 2px solid #08cc99;
  display:flex;
  align-items: baseline;
  background-color: #ecf8f6;
  padding: 16px 20px;
  margin: 20px 0 !important;
}

ol.install-steps li::before {
  content: counter(my-awesome-counter) ". ";
  color: #44d2ab;
  font-weight: bold;
  margin-right: 10px;
  font-size: 22px;
}


ol.install-steps li p {
  display: inline;
  margin: 0 !important;
  font-size: 18px !important;
}

ol.install-steps li code {
    font-size: 16px !important;
    display: block;
    margin: 0px 0 !important;
    padding: 6px !important;
    background-color: rgb(42 42 53 / 95%) !important;
    color: white !important;
    width: 270px;
    margin-top: 6px !important;
}

ol span.geshifilter {
    display: inherit;
}

</style>

## Installation modes

### Web Installer

If your machine is local or web accessible, a temporary web server will start on TCP port [:8080], providing a wizard
for configuring basic settings.

[:image:1_quick_start/installation/web-installer.gif]

After it completes, the server restarts automatically and you are good to go.

### Command line Installer

If you would rather use a shell, you can perform the same steps using command-line (cli) prompts.

[:image:1_quick_start/installation/cli-installer.gif]


## Installation Troubleshooting

### Cannot access to web interface

Make sure the Cells binding port is properly open for TCP connections in your firewall or equivalent (e.g. Security Group in Amazon AWS).
Default port is 8080 (or another available http-alt port like 8008, 8081, ...).

Check the logs to verify which port to open:

```
2021-01-27T11:17:00.248+0100	INFO	Activating privacy features... done.
2021-01-27T11:17:00.278+0100	INFO	https://0.0.0.0:8080
```

### SQL Errors

_After start, you see a bunch of errors starting with: `ERROR   pydio.grpc.meta   Failed to init DB provider   {"error": "Error 1071: Specified key was too long; max key length is 767 bytes handling data_meta_0.1.sql"}`_.

You might have an unsupported version of the mysql server: you should use MySQL server version 5.7 or higher or MariaDB version 10.3 or higher.

### GLIBC [Linux]

_You see this error: `/lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.14' not found`_

The version of libc6 is outdated. Run these commands to upgrade it.

```sh
sudo apt-get update
sudo apt-get install libc6
```

### SELinux [CentOS]

_The main application page in your browser displays the following error: `Access denied.`_

Ensure you have modified SELinux to be disabled or running in permissive mode. To temporary disable SELinux: `sudo setenforce 0`.

You can also permanently disable SELinux in `/etc/selinux/config`.
