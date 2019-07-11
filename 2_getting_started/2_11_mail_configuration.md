In this chapter, we have a look at the mailing system integrated to Pydio Cells and guide you through some sample configurations.

**Please Note**: this configuration requires a **restart of the server** to be properly taken into account.

## Configure Mails

To configure the mailer, go to **Application Parameters > Mailers**:

Field | Description
--- | ---
Default From Email address | put the email that you're going to use
Mailer engine  | What type of engine you're going to use to send the mails you will have 3 choices we are going to explain all of them and show you a configuration sample.

Depending on the engine you have chosen, you see one of the following form:

### 1. SMTP Server

Field | Description
--- | ---
Server Hostname  | the SMTP server's hostname, *for example gmail's smtp is `smtp.gmail.com`*
Server Port  |  The port of the SMTP server, *for example gmail's default port is `587`*
Connection User  | The username of the address used to connect to this SMTP server, such as *john.doe@gmail.com, john.doe@pydio.com*
Connection Password  | password of the user above
Queue Type  |  Right now do not modify this field.

### 2. Sendmail

In order to use sendmail:

- Make sure that you have installed and configured it on your server
- (Depending on which version you are using), make also sure that the `/usr/bin/sendmail` path is correct.
- For more security and to protect the field from getting exposed or used with paths that should not be accessible, you can configure the path for the mailer inside the Pydio.json file located in `~/.config/pydio/cells/<here>`, look for this part and put the where your mailer is:

```json
    "pydio.grpc.mailer": {
      "queue": {
        "@value": "boltdb"
      },
      "sender": {
        "@value": "sendmail",
        "executable": "<your value>"
      }
```

### 3. Sendgrid

To use sendgrid, you only have to:

- Retrieve your specific **API key**
- Put it in the corresponding field.


## [Enterprise Edition] Custom templates for mailer

With the Enterprise Edition there are 3 additional options that will allow you to change the mailer template:

* Change the Logo
* Change the Application Name
* Change the Footer (Usually copyrights)

[:image-popup:2_getting_started/mailer_template_edition_example.png]

_Refer to the numbers on the screenshot for the list below_

1. To modify the Logo you must go to **Advanced Settings > UI Customization** and modify **Emails Template Logo** with an url pointing to an image directly, for instance `http://domain/image.png`.

2. To modify the application name go to **Application Core > Main Options** and change the **Application Title** setting.

3. To modify the footer (bottom text) go to **Advanced Settings > UI Customization** and modify **Emails Template Footer**.
