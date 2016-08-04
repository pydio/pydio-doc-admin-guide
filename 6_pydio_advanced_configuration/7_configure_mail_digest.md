Since [Pydio 6.4.0](https://pydio.com/fr/node/1279), we add the "Mail digest" feature to avoid receiving tons of emails when watching for a file or folder modification.

This page is here to help you to configure the sending mail group.

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

##Configure the diget in My Profile

Go to "My Account" in the top right of your Pydio and configure your own digest frequency

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_4.png]

Don't forget to save before leaving the page

##Enable and configure the CRON

We configure the digest mail but the action who send the grouped email, **consume_mail_queue**, need to be called to work. That's why we gonna create a scheduler or CRONTAB in our Pydio. The scheduler gonna call the action with some frequency we get chosen. But first, we need to enable the scheduler. 

We need to go to "Setting" page and at the bottom left "Available Plugins" > "Action Plugins" > "Tasks Scheduler"

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_5.png]

If the logo scheduler was not in the bottom left, you should forget to enable the "Command-line Active" in the "Application Core" Section

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_6.png]

##Create the CRON

Now we can create a CRON when you click to the "Scheduler" icon and to "NEW TASKS"

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_7.png]

+ Label : The name of the task ( usefull when you have many CRON to find the good one easily)   

+ Schedule : The CRONTAB expression, maybe the hardest part to fill. The format is Minutes Hours Day Week Month. If you don't know how to do it, you can go at the end of the doc to see some exmples   

+ User(s): The user who gonna do the action, can be a list of users ( separated with comma or with * )   

+ Repository ID: The id of the workspaces, in our case we can choose "All Repositories"   

Now we need to choose the action and his parameters by clicking to "Parameters" tab

+ Action : Obviously, it's the action who the CRON gonna do, they are sorted by the plugin so you need to go to "core.mailr" and to "consume_mail_queue"   

Param Name and Param Value can be empty for this action.   
Click to "OK" to validate the CRON.

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_8.png]

You can click to the play button on the right of every CRON to start them.

You CRON for the mail digest is now complete.

If you get some notification, you gonna get an email like this :

[:image-popup:6_pydio_advanced_configuration/config_mail_digest_9.png]

We can see here that the layout was successfully edited.

##CRON expression

Here are some exemple of CRON expression :

+ ` */5 * * * *` : Every 5 minutes   

+ `30 14 * *` : At 14:30 every day   

+ `00 12 * 3` : At 12:00 every Wednesday   

+ `25 12 12 4 *` : At 12:25 on the 12th in April   

Or you can use [this tool](http://crontab.guru/)
