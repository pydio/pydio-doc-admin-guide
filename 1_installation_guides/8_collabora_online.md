**TODO**

### Configure Collabora Online Plugin

Collabora Online is a powerful LibreOffice-based online office suite which supports all major document, spreadsheet and presentation file formats:​ DOC, DOCX, PPT, PPTX, XLS, XLS, ODF, ODS, ODP.  

It also supports shared editing, meaning that while one person edit a document, others see the changes in real time. Different persons can make changes at any given time.

This documentation describes how to deploy the Collabora CODE docker image (community edition). Please contact us if you want more details about what contains or how to use the professional edition.

#### Installing Collabora Online Development Environment via the Docker Image

##### Prerequisite 

In this documentation, you have to replace <your-domain> with the name of the domain where your Pydio Cells installation is setup.

You also have to replace <your-escaped-domain> with the same subdomain where dots are "escaped", eg. office\.yourdomain\.com

Here we will give you an example on one configuration if you want more detail about collabora you can go **[Here](https://www.collaboraoffice.com/code/)**.

#### Start Docker Image
You can find a link to the docker for collabora **[here](https://hub.docker.com/r/collabora/code/)**

Install Docker on a server. 

    $ docker pull collabora/code
    $ docker run -t -d -p 127.0.0.1:9980:9980 -e "domain=<your-escaped-domain>" --cap-add MKNOD collabora/code

Note: this docker image does not work on Ubuntu 14.04 LTS, because Ubuntu 14.04 LTS has missing kernel compile option CONFIG_AUFS_XATTR=y, which is leading to setcap not working on docker’s aufs storage. Upstream bug: https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1557776

## Configuring Pydio Plugin to connect to CODE

In the Settings panel, go to All Plugins > Features Plugins > Editors and enable the Collabora Online plugin. 

[:image-popup:1_installation_guide/collabora-enterprise.png]

Edit its parameters as follow: 

 - Url to the Libre Office Editor iFrame: `/loleaflet/dist/loleaflet.html`
 - WebSocket use TLS: `true`
 - Web Socket Connector Host: `<your-domain>`
 - Web Socket Connector Port: `9980` (see virtual host configuration above).
 
## Test and start editing docs ! 
 
Switch to a workspace and use the "New Folder" > "You can also create an empty Document" link to create e.g. an ODT Document.

Double click the new file to edit, you should now be able to edit it directly in Pydio!

[:image-popup:1_installation_guide/collabora-test.png]
