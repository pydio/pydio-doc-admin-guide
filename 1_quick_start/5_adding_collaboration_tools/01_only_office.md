# Only Office plugin for Cells - Enterprise Distribution

ONLYOFFICE is a multifunctional portal for business collaboration, document and project management. It allows you to organize business tasks and milestones, store and share your corporate or personal documents, use social networking tools such as blogs and forums, as well as communicate with your team members via corporate IM.

It supports all type of formats such as DOC, DOCX, PPT, PPTX, XLS, XLS, ODF, ODS, ODP and also has the best best support of microsoft editors features like WORD, POWERPOINT, EXCEL and so on.

## Install and enable Only Office Plugin in a running Cells Enterprise Distribution instance

### Get your OnlyOffice instance

You can retrieve OnlyOffice on their offical website **[right here](https://www.onlyoffice.com/compare-editions.aspx)**.
You will find all the details and different features between all the editions, for small enterprises (up to 50 users simultaneous connection) we recommand the [*integration edition start*](https://www.onlyoffice.com/connectors.aspx).

### Enable the plugin

To enable the plugin, go to `Application parameters > Available Plugins > Only Office`, then:
 
- Only office tls: `true` if you want to secure communication between OnlyOffice and pydio or `false`
- Only office Host: _`<host where OnlyOffice is running>`_
- Only office Port: _`<OnlyOffice instance port>`_ usually it is running on `9980`

[:image:1_quick_start/office_online/ent_only_office_plugin.png]

#### OnlyOffice docker image

There is an [OnlyOffice docker image](https://hub.docker.com/r/onlyoffice/documentserver/),
the image only offers the features of the *[community edition](https://www.onlyoffice.com/compare-editions.aspx)*.

The container's default port is 80 so redirect it to another port - by default we use 9980 in the Pydio Cells config.

```shell
docker run -i -t -d -p 9980:80 --restart=always onlyoffice/documentserver
```

Note: refer to docker.io web site to install correct version of docker. Typically, on Ubuntu, you should install docker using `apt install docker.io`, the simple `docker` package installs something else that is irrelevant here.

### Start editing documents

Now you can edit all of your docs, presentations, and more easily, double click on the file and a window will appear with the only office interface and then you can edit it right off the bat.

[:image-popup:1_quick_start/office_online/ent_onlyoffice_interface.png]

### Specific configs of onlyoffice service

We have noticed that documents are only pushed back to Pydio Cells, when _everybody_ closes the opened document.
This can lead to nasty behaviour and data loss.

You should configure your docker image manually to workaround this (for further details, please refer to the [OnlyOffice online documentation](https://api.onlyoffice.com/editors/save)) by performing the following:

- Ssh to the target server

```sh
# Retrieve the id of the correct container
$ docker ps
CONTAINER ID....
c263411ad1d0       onlyoffice/documentserver
```

- Log into the container

```sh
docker exec -it c2 /bin/sh

# edit local.json file
nano /etc/onlyoffice/documentserver/local.json
# add this under services/CoAuthoring 
    "autoAssembly": {
        "enable": true,
        "interval": "5m"
     },

# save, exit
# restart the container and you should be good to go
```

For the record, the begining of the file should look like this:

```json
{
 "services": {
   "CoAuthoring": {
     "autoAssembly": {
       "enable": true,
       "interval": "5m"
     },
     "sql": {
     ...
```

You can validate that your change has been successful by leaving a modified OO document opened in a tab and check (even better with another user) that the modification date of the corresponding file changes.
