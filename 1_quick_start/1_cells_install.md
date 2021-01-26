Get started with Cells static binaries

<ol class="install-steps">
<li><p><strong>Get your database access ready</strong> and login to your server as a dedicated user (see <a href="./requirements'">Requirements</a>)</p></li>
<li><p><strong>Download the binary</strong> for your server architecture from the <a href="/en/download">Download Page</a>. 
    <br>On Linux/Mac, <strong>make binary executable </strong> with::<br> <code>$ chmod +x cells</code></p></li>
<li><p><strong>Go through Cells configuration</strong> steps using either a browser or the command line:<br> <code>$ ./cells configure</code></p></li>
<li><p><strong>Start Cells</strong> : web installer mode will restart automatically after configuration, otherwise use: <code>$ ./cells start</code></p></li>
<li><p><strong>Open your web browser</strong> at <a href="https://localhost:8080">https://localhost:8080</a> <br> (or https://[server ip or domain]:8080).</p></li>
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

If you are on a desktop machine, the installer opens a web page with a wizard asking you to fill in various configuration parameters, including DB connection info and the login/password of the main administrator.

Click on the image below to see a screencast of the installation:

[:image:1_quick_start/installation/web-installer.gif]

After it completes, the server restarts automatically and you are good to go.

### Command line Installer

If you are more a shell person, you can perform the same steps using command-line (cli) prompts/answers. 
The image below shows a screencast of the CLI installation :

[:image:1_quick_start/installation/cli-installer.gif]



## Troubleshooting

#### Connection URL 

The Cells gateway starts by default on `0.0.0.0:8080`.

By using 0.0.0.0, the gateway will allow incoming traffic to any IPv4/domain of the machine network interfaces.

If the port 8080 is busy (already used by another process), it will try other options.

A self-signed certificate is also automatically generated for supporting TLS connection out of the box (https).

For production use, or for a remotely accessed Cells, you can make the gateway listen to multiple ports on multiple IP addresses,
each with its own set of rules attached (TLS, Let's Encrypt, External URL)

 - At installation time using specific flags (`--bind`, `tls_xxx`): see the [man page of the configure command TODO](LINK TO DOC). 
 - With the dedicated tool `./cells configure sites`: see this [dedicated page for more information about the "sites" (TODO)](LINK TO PAGE).