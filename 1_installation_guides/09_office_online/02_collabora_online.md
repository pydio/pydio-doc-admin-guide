
Collabora Online is a powerful LibreOffice-based online office suite which supports all major document, spreadsheet and presentation file formats:​ DOC, DOCX, PPT, PPTX, XLS, XLS, ODF, ODS, ODP.

It also supports shared editing, meaning that while one person edits a document, others see the changes in real time. Different persons can make changes at any given time.

This documentation describes how to deploy the Collabora CODE docker image (community edition). Please contact us if you want more details about what contains or how to use the professional edition.

## Install Collabora Online Development Environment

We only provide on this page an example of configuration that meets the basic requirements for the plugin to work with Pydio Cells. If you want more detail about the different configurations Collabora offers, you can visit **[this link](https://www.collaboraoffice.com/code/)**.

### Package installation

You can install CODE on the same server as your pydio or on a another server, it's all up to you.
The process is quite simple you can follow the [CODE documentation](https://www.collaboraoffice.com/code/linux-packages/).
Depending on your system the commands will differ but it should work at the end,
then you can configure the plugin to match the location of your CODE instance (host, port, ssl), the instructions are below.

### CODE Docker image

**[Install Docker](https://docs.docker.com/install/)** on a server.

```shell
docker pull collabora/code
docker run -t -d -p 9980:9980 -e "extra_params=-o:ssl.enable=false" collabora/code
```
_for testing puposes ssl is disabled, but we advise you to always have it on_

You can find all the details on the docker image such as the env variables, etc... **[Docker CODE offical documentation](https://www.collaboraoffice.com/code/docker/)**.


### Configure Pydio Plugin to connect to CODE

Go to `Settings > All Plugins > Features Plugins > Editors` and enable the Collabora Online plugin.

Change the plugin parameters to:

- Libre office ssl: `true` if you want secured communication between collabora and pydio or `false`
- Libre office Host: _`<host where CODE is running>`_
- Libre office Port: _`<CODE instance port>`_ usually it is running on `9980`

Remember to save.

### Test and start editing docs

Switch to a workspace and use the "New Folder" > "You can also create an empty Document" link to create e.g. an ODT Document.

Double click on the new file to edit, you are now able to edit it directly in Pydio Cells using collabora online editor!
