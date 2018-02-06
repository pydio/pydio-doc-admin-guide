Since [Pydio 6.4.0](https://pydio.com/fr/node/1279), we added the "Mail digest" feature to avoid receiving tons of emails when watching for a file or folder modification.

This page is here to help you configure the mail digest and the mail sender. The mail sender is required to send digest emails.

##Enable the feature

You need to go to your Setting page, then click to the mail icon in your left and enable the feature by clicking in the "Activate Queue" field.

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_0.png]

##Configure the Mailer Plugin

Now we need to configure the Mailer plugin to avoid any problem when Pydio send a mail.
I take the easiest exemple by using the smtp of Gmail with PHPMailer

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_1.png]

+ Mailer Plugin : I choose "PHPMailer" because I use the smtp ( who's disabled in "PHPMailer-lite")   

+ Mailer : The mail software, "smtp" is selected in this exemple   

When you choose "smtp" as mail software, several fields are added :   

+ Host : The host of the smtp, for gmail it's "smtp.gmail.com"   

+ Port : The port number of the Host, gmail use the port 587   

+ SMTP Authentification : If you need to add your login / password to use the smtp, select "YES"   

+ Username : In many case, it's your email   

+ Password : The password of your email   

You also need to configure the sender when Pydio send a mail.

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_2.png]

+ Sender Mail : The mail of the sender ( generaly the same as your email )   

+ Sender Name : The name of the sender ( can be your real name or a bot name such as "Pydio-bot" )   

+ Unique Sender : You can force Pydio to use only the "Sender Mail" to avoid any problem   

It's optionnal but you can also configure the content of the mail

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_3.png]

Subject Prepend : Before the subject of Pydio, you can add a prefix so the mail can be more visible   

+ Subject Append : After the subject of Pydio, you can add a sufix   

+ Body Layout : The body of the mail send by Pydio, you can add HTML to change the template ( DO NOT DELETE AJXP_MAIL_BODY )   

+ Layout Folder : The folder where are the layout   

If you're going to use another configuration you can find links to guides **[here](https://pydio.com/en/docs/kb/plugins/setting-emailers)**.

##Configure the diget in My Profile

Go to "My Account" in the top right of your Pydio and configure your own digest frequency

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_account.png]

Don't forget to save before leaving the page

##Enable and configure the CRON

We configured the digest mail, but we need to activate an action to send the mail. The action who send the grouped email, **consume_mail_queue**, need to be called to work. That's why we gonna create a scheduler or CRONTAB in our Pydio. The scheduler gonna call the action with some frequency we get chosen. But first, we need to enable the scheduler. 

We need to go to "Setting" page and at the bottom left **Application Parameters > Scheduler**.

##Create the CRON

You can go to this how-to that will explain to you how to use the CRON & Schedule tasks.

https://pydio.com/en/docs/kb/plugins/using-tasks-scheduler
