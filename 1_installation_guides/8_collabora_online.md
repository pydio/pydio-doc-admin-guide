### Configure Collabora Online Plugin

Collabora Online is a powerful LibreOffice-based online office suite which supports all major document, spreadsheet and presentation file formats:​ DOC, DOCX, PPT, PPTX, XLS, XLS, ODF, ODS, ODP.  

It also supports shared editing, meaning that while one person edit a document, others see the changes in real time. Different persons can make changes at any given time.

This documentation describes how to deploy the Collabora CODE docker image (community edition). Please contact us if you want more details about what contains or how to use the professional edition.

#### Installing Collabora Online Development Environment via the Docker Image

##### Prerequisite 

In this documentation, you have to replace _&lt;your-domain&gt;_ with the name of the domain where your Pydio Cells installation is setup.

You also have to replace _&lt;your-escaped-domain&gt;_ with the same subdomain where dots are "escaped", eg. office\.yourdomain\.com

We only provide on this page an example of configuration that meet the basic requirements for the plugin to work with Pydio Cells. If you want more detail about the different configurations Collabora offer, you can visit **[this link](https://www.collaboraoffice.com/code/)**.

##### Start Docker Image

**[Install Docker](https://docs.docker.com/install/)** on a server. 

    $ docker pull collabora/code
    $ docker run -t -d -p 127.0.0.1:9980:9980 -e "domain=<your-escaped-domain>" --cap-add MKNOD collabora/code

Note: this docker image does not work on Ubuntu 14.04 LTS, because Ubuntu 14.04 LTS has missing kernel compile option CONFIG_AUFS_XATTR=y, which is leading to setcap not working on docker’s aufs storage. Upstream bug: https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1557776

##### Configuring Pydio Plugin to connect to CODE

Go to Settings > All Plugins > Features Plugins > Editors and enable the Collabora Online plugin. 

Change the plugin parameters to : 

 - Url to the Libre Office Editor iFrame: `/loleaflet/dist/loleaflet.html`
 - WebSocket use TLS: `true`
 - Web Socket Connector Host: `<your-domain>`
 - Web Socket Connector Port: `9980` (see virtual host configuration above).
 
 Remember to save.
 
##### Test and start editing docs ! 
 
Switch to a workspace and use the "New Folder" > "You can also create an empty Document" link to create e.g. an ODT Document.

Double click the new file to edit, you should now be able to edit it directly in Pydio!
