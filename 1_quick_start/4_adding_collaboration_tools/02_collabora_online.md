
Collabora Online is a powerful LibreOffice-based online office suite which supports all major document, spreadsheet and presentation file formats:​ DOC, DOCX, PPT, PPTX, XLS, XLS, ODF, ODS, ODP.

It also supports shared editing, meaning that while one person edits a document, others see the changes in real time. Different people can make changes at any given time.

This documentation describes how to deploy the Collabora CODE docker image (community edition). Please contact us if you want information about the professional edition.

## Install Collabora Online Development Environment

On this page is a configuration example that meets the basic requirements for the plugin to work with Pydio Cells. If you want more detail about the different configurations Collabora offers, you can visit **[this link](https://www.collaboraoffice.com/code/)**.

### Package installation

You can install CODE on the same server as your Pydio or on another server, it's up to you.
The process is quite simple and you can follow the [CODE documentation](https://www.collaboraoffice.com/code/linux-packages/).
The commands will differ depending on your system but it should work at the end. Then you can use the instructions below to configure the plugin to match the location of your CODE instance (host, port, ssl).

### CODE Docker image

**[Install Docker](https://docs.docker.com/install/)** on a server.

```shell
docker pull collabora/code
docker run -t -d -p 9980:9980 -e "extra_params=-o:ssl.enable=false" collabora/code
```
_for testing puposes ssl is disabled, but we advise you to always have it on_

You can find all of the information about the docker image, such as the env variables, etc... **[Docker CODE official documentation](https://www.collaboraoffice.com/code/docker/)**.


### Configure Pydio Plugin to connect to CODE

Go to `Cells Console > Application Parameters > All Plugins` and enable the Collabora Online plugin.

Change the plugin parameters to:

- Libre office ssl: `true` if you want secured communication between collabora and pydio or `false`
- Libre office Host: _`<host where CODE is running>`_
- Libre office Port: _`<CODE instance port>`_ usually it is running on `9980`

Remember to save.

### Test and start editing docs

Switch to a workspace and right-click to display a pop-up menu from which you can create a new document.

Double-click on the new file to edit. You are now able to edit it directly in Pydio Cells using the Collabora online editor!

[:image-popup:1_quick_start/office_online/collabora_interface.png]
