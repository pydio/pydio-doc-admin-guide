
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v2. Looking for <a href="https://pydio.com/en/docs/cells/v3/quick-start">Pydio Cells v3 docs?</a></span>
</div>




In this chapter, we have a look at the mailing system integrated to Pydio Cells and guide you through some sample configurations.

**Please Note**: this configuration requires a **restart of the server** to be properly taken into account.

## Configure Mails

To configure the mailer, go to **Application Parameters > Mailers**:

| Field                               | Description                                                                                                                                               |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Default From Email address          | Put the email that you're going to use as sender address for notifications and as default (see below).                                                    |
| From/Sender address and name to use | Select how the address and display name is chosen. This setting controls how the envelope from address and the from mail and sender mail headers are set. |

Depending on the engine you have chosen, you see one of the following form:

### 1. SMTP Server

| Field               | Description                                                                                                       |
| ------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Server Hostname     | the SMTP server's hostname, *for example gmail's smtp is `smtp.gmail.com`*                                        |
| Server Port         | The port of the SMTP server, *for example gmail's default port is `587`*                                          |
| Connection User     | The username of the address used to connect to this SMTP server, such as *john.doe@gmail.com, john.doe@pydio.com* |
| Connection Password | password of the user above                                                                                        |
| Queue Type          | Right now do not modify this field.                                                                               |

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
