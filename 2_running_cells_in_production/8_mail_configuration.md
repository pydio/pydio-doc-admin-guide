In this chapter, we have a look at the mailing system integrated to Pydio Cells and guide you through some sample configurations.

**Please note:** when changing these configurations, the assiociated `pydio.grpc.mailer` service will be automatically restarted, so the changes may not be impacted until after 10 to 20 seconds.
At restart time, service tries to dial a network connection to the specific mailing service, and if something goes wrong, the application will be aware that there is no valid mailer configured.

## Configure Mails

To configure the mailer, go to **Application Parameters > Mailers**:

| Field                               | Description                                                                                                                                               |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Default From Email address          | Put the email that you're going to use as sender address for notifications and as default (see below).                                                    |
| From/Sender address and name to use | Select how the address and display name is chosen. This setting controls how the envelope from address and the from mail and sender mail headers are set. |

Depending on the engine you have chosen, you see one of the following form:

### 1. Disable Mailer

The default value, no mailer is configured. Application will never try to send emails.

### 2. No-Op Mailer

By selecting this mailer, emails will be created by the application but they will just be collected without further processing.
This can be handy typically for **staging servers** where you want to act as if a mailer is enabled, but make sure that your users
never receive actual emails. 

This mailer "honeypot" can either fully discard emails, or write them as text to a file for debugging purpose.

### 3. SMTP Server

| Field               | Description                                                                                                       |
| ------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Server Hostname     | the SMTP server's hostname, *for example gmail's smtp is `smtp.gmail.com`*                                        |
| Server Port         | The port of the SMTP server, *for example gmail's default port is `587`*                                          |
| Connection User     | The username of the address used to connect to this SMTP server, such as *john.doe@gmail.com, john.doe@pydio.com* |
| Connection Password | password of the user above                                                                                        |
| Queue Type          | Right now do not modify this field.                                                                               |

### 4. Sendmail

In order to use sendmail:

 - Make sure that you have installed and configured it on your server
 - The sendmail executable is set by default to `/usr/bin/sendmail`. Depending on your system and its version, you may have to adapt this executable. For security reasons, this **cannot be done directly from the interface** but must be manually edited inside the main configuration file. To achieve that, open `CELLS_WORKING_DIR/pydio.json` and look for the "pydio.grpc.mailer" section. 

```json
 "services": {
  //...
  "pydio.grpc.mailer": {
    "queue": {
      "@value": "boltdb"
    },
    "sender": {
      "@value": "sendmail",
      "executable": "/path/to/custom/sendmail"  // Add this key directly in the file
    }
  },
  //...
}   
```

### 5. Sendgrid

To use SendGrid API Service, you only have to:

- Retrieve your specific **API key**
- Put it in the corresponding field.
