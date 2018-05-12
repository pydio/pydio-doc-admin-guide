For this chapter we will have a look at the mailing system integrated to Pydio Cells, we will see the different means of configuration.

### Configure Mails

To configure the mailer go ** Application Parameters > Mailers** :

Field | Description
--- | ---
Default From Email address | put the email that you're going to use
Mailer engine  | What type of engine you're going to use to send the mails you will have 3 choices we are going to explain all of them and show you a configuration sample.

1. **SMTP Server**

Field | Description
--- | ---
Server Hostname  | the SMTP server's hostname, *for example gmail's smtp is `smtp.gmail.com`*
Server Port  |  The port of the SMTP server, *for example gmail's default port is `465`*
Connection User  | The username of the google address that you're going to use, such * john.doe@gmail.com, john.doe@pydio.com (as long as it's an email from google's services)*
Connection Password  | password of the user above
Queue Type  |  How you want Pydio to handle the mailing either when it's created or queue it and send it at once.

2. **Sendmail**

For sendmail make sure that you have installed and configured it on your server (depending on which version you are using), make also sure that the `/usr/bin/sendmail` path is correct, then choose the queue type as it's explained above.

3. **Send Grid**

For sendgrid get the **api key** and put it in that field after that all the mailing will be handled. For the queue type refer to the 1st paragraph.
