<div style="background-color: #fbe9b7;font-size: 16px;">
<span style="background-color: #fae4a6;padding: 10px;font-family: FuturaT-Demi;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Pydio 7. Looking for <a href="https://pydio.com/en/docs/v8/watches-and-notifications">Pydio 8 docs?</a></span>
</div>

### Introduction
Notification is a powerful feature of Pydio, once activated through a specific plugin, and as of v5, part of the core. It will allow the publication of two kind of objects :

+ **Alerts:** based on a user **_voluntary opt-in_**, alerts can send email when the content of a file or folder is either modified or consulted.
+ **Event feeds:** to reflect the "life" of the data, the event feed lists all actions triggered in all workspaces that the user can access.

Those two could be compared respectively to Facebook "Notifications" and "Feed".

### Basic installation
Activating notifications used to be quite cumbersome, but the procedure was greatly simplified with the introduction of the application installer. Basically, this feature requires an SQL database (for the moment, no-sql implementation are in the way). Still, this can be as simple as specifying an Sqlite file, so you don't really need a MySql running somewhere.

+ Configure an SQL Connexion: if not already done, in the Application Parameters > Configs Backends.
+ Activate the "User events and alerts" in the Notification Center (Application Parameters > Notifications). There is only one plugin available, so it's selected by default (Events SQL). Click on the Install SQL Tables button to make sure ajxp_feed table is created.
+ **_This will already be enough to enable the Events Feeds.  But you will also want to let the user watch for a given folder to receive alerts :_**
+ Add a "Meta.watch" feature to each workspace
+ Make sure the emailer is correctly setup by checking the various parameters of the Application Parameters > Mailers panel. Warning, either through the administrator Email, or via the "Sender Email" field of this panel, you must make sure that an email is configured to fill the "From" value of the emails, otherwise they won't be sent correctly. Also, this is your mission to correctly configure your PHP to send emails.

Once all this is active, switch to any workspace, you should see the "Notifications" button appear in the top toolbar. If you right-click on a file or folder, you should as well see the "Watch... " menu allowing you to flag the selected item as watched.

### Optimizing performances
#### _Disable output buffering!_

Make sure that output buffering is disable for your PHP configuration. This allows Pydio to register some actions to be performed AFTER the pages are render to the client browser. Events feeds can be time consuming, as every action on the data must be logged and analysed to see if it must trigger an alert. When the system is correctly configured, this is done at the very end of the scripts, after rendering results of the queries for the clients.

This is not recommended only for this feature, many features implying metadata update or indexation are also using this system to avoid lowering your system response celerity.

#### _Messages Queuing_

For system under high load (high number of users basically), you should consider switching on the "Messages Queuing" mechanism. This will allow the events to be treated asynchronously : all events are stored in a queue, and you will have to setup a CRON job to consume the queue on a regular basis.

You will have enable the CLI version of the framework, and then launch the following command, for example every 5 minutes:

`php /path/to/ajxp/cmd.php -r=ajxp_conf -u=admin -p=adminpassword -a=consume_notification_queue`
