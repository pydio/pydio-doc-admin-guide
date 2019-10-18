# Only Office plugin for Cells - Enterprise Distribution

ONLYOFFICE is a multifunctional portal for business collaboration, document and project management. It allows you to organize business tasks and milestones, store and share your corporate or personal documents, use social networking tools such as blogs and forums, as well as communicate with your team members via corporate IM.

It supports all type of formats such as DOC, DOCX, PPT, PPTX, XLS, XLS, ODF, ODS, ODP and also has the best best support of microsoft editors features like WORD, POWERPOINT, EXCEL and so on.

## Install and enable Only Office Plugin in a running Cells Enterprise Distribution instance

### Get your OnlyOffice instance

You can retrieve OnlyOffice on their offical website **[right here](https://www.onlyoffice.com/compare-editions.aspx)**.
You will find all the details and different features between all the editions, for small enterprises (up to 50 users simultaneous connection) we recommand the [*integration edition start*](https://www.onlyoffice.com/connectors.aspx).

### Enable the plugin

To enable the plugin, go to `Application parameters > Available Plugins > Only Office`, then:
 
- Only office ssl: `true` if you want to secure communication between OnlyOffice and pydio or `false`
- Only office Host: _`<host where OnlyOffice is running>`_
- Only office Port: _`<OnlyOffice instance port>`_ usually it is running on `9980`

[:image-popup:1_installation_guides/office_online/only_office_plugin.png]

#### OnlyOffice docker image

There is an [OnlyOffice docker image](https://hub.docker.com/r/onlyoffice/documentserver/),
the image only offers the features of the *[community edition](https://www.onlyoffice.com/compare-editions.aspx)*.

The container's default port is 80 so redirect it to another port - by default we use 9980 in the Pydio Cells config

```shell
docker run -i -t -d -p 9980:80 --restart=always onlyoffice/documentserver
```

Note: refer to docker.io web site to install correct version of docker. Typically, on Ubuntu, you should install docker using `apt install docker.io`, the simple `docker` package installs something else that is irrelevant here.

### Start editing documents

Now you can edit all of your docs, presentations, and more easily, double click on the file and a window will appear with the only office interface and then you can edit it right off the bat.

[:image-popup:1_installation_guides/office_online/onlyoffice_interface.png]

### Specific configs of onlyoffice service

If you have problem in saving file, there are several options in config of onlyoffice service that you may change to match your server. They can be found in this link: https://api.onlyoffice.com/editors/save