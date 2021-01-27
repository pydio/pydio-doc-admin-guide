_Get started quickly with Cells static binaries._

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
