ONLYOFFICE is a multifunctional portal for business collaboration, document and project management. It allows you to organize business tasks and milestones, store and share your corporate or personal documents, use social networking tools such as blogs and forums, as well as communicate with your team members via corporate IM.

It supports all type of formats such as DOC, DOCX, PPT, PPTX, XLS, XLS, ODF, ODS, ODP and also has the best best support of microsoft editors features like WORD, POWERPOINT, EXCEL and so on.

## Install and enable Only Office Plugin in a running Cells Enterprise Distribution instance

### Get your OnlyOffice instance

You can retrieve OnlyOffice on their offical website **[right here](https://www.onlyoffice.com/compare-editions.aspx)**.
You will find all the details and different features between all the editions, for small enterprises (up to 50 users simultaneous connection) we recommand the [*integration edition start*](https://www.onlyoffice.com/connectors.aspx).

#### OnlyOffice docker image

For a quick start, you can use the official [OnlyOffice docker image](https://hub.docker.com/r/onlyoffice/documentserver/): yet this image only offers the features of the *[community edition](https://www.onlyoffice.com/compare-editions.aspx)*.

Simply run:

```shell
docker run -i -t -d -p 9980:80 -e JWT_ENABLED=false --restart=always onlyoffice/documentserver
```

Note that to avoid collision with the main entry point of your Cells Server, we bind the container default port (80) to another free port.
In this document, we use port 9980 as an example.

### Enable the plugin

To enable the plugin in the Cells Admin Console, go to `Application parameters > Available Plugins > Only Office`, then configure following parameters:
 
- Only office TLS: `true` if you want to secure communication between OnlyOffice and Pydio or `false` otherwise
- Only office Host: _`<host where OnlyOffice is running>`_
- Only office Port: _`<OnlyOffice instance port>`_, we commonly use `9980`

[:image:1_quick_start/office_online/ent_only_office_plugin.png]

### Start editing documents

Now you can edit all of your docs, presentations, and more easily, double click on the file and a window will appear with the only office interface and then you can edit it right off the bat.

[:image-popup:1_quick_start/office_online/ent_onlyoffice_interface.png]

## Troubleshooting

### JWT Token Error

After upgrading OnlyOffice to version >= 7.2, you may encounter a problem with the security token:  
when trying to open a document, you see an error popup with a _Json WebToken Error_ or similar. If you do not already pass the JWT_ENABLED=false environment variable at startup, you can disable this via the configuration file.

- Edit the OnlyOffice configuration file that can be found at:

`/etc/onlyoffice/documentserver/local.json`

- Set all **3** values of token parameters to "false"

```json
{
  "services": {
    "CoAuthoring": {      
      "token": {
        "enable": {
          "request": {
            "inbox": false,
            "outbox": false
          },
          "browser": false
        },
```

- Restart the service. If you are using the OnlyOffice docker image, the easiest way is to restart the services while still inside the image:

`supervisorctl restart all`

You can find more info on this in the [Only Office knowledge base](https://helpcenter.onlyoffice.com/installation/docs-configure-jwt.aspx).

### Changes seems to be missed after save

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
docker exec -it c2 /bin/bash

# edit local.json file
nano /etc/onlyoffice/documentserver/local.json
# add this under services/CoAuthoring 
    "autoAssembly": {
        "enable": true,
        "interval": "5m"
     },

# save, exit
```

- Restart the container (or the OO services while you are still inside the container see above) and you should be good to go.


For the record, the begining of the configuration file should then look like this:

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
