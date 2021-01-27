_Get started quickly with Cells static binaries. You can also use [Docker container](./docker) or
[Cloud Images](./cloud-images) (AMI/VMWare/OVF, Cells Enterprise only) or refer to dedicated [OS-specific step-by-step tutorials](/en/docs/kb/deployment)._

<ol class="install-steps">
<li><p><strong>Get your database access ready</strong> and login to your server (see <a href="./requirements">Requirements</a>).</p></li>
<li><p><strong>Download the binary</strong> for your server architecture from the <a href="/en/download" target="_blank">Download Page</a>. 
    <br>On Linux/Mac, <strong>make binary executable </strong> with:<br> <code>$ chmod +x ./cells</code></p></li>
<li><p><strong>Go through Cells configuration</strong> steps (see Installation Modes below):<br> <code>$ ./cells configure</code></p></li>
<li><p><strong>Start Cells</strong> : web installer mode will restart automatically after configuration, otherwise use: <code>$ ./cells start</code></p></li>
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

If your machine is local or web accessible, a temporary web server will start on TCP port [:8080], providing a nice wizard
for setting basic configurations.

[:image:1_quick_start/installation/web-installer.gif]

After it completes, the server restarts automatically and you are good to go.

### Command line Installer

If you are more a shell person, you can perform the same steps using command-line (cli) prompts/answers. 

[:image:1_quick_start/installation/cli-installer.gif]
