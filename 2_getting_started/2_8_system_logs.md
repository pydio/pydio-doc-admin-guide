In this chapter we are going to have a close look at the logs and where they are stored and what is stored in them.

### System Logs

With pydio cells you have a logging system allowing you to always be informed about what's happening. The logs are storing every event happening on pydio cells such as a user creation, access, errors and more.

#### Enabling logs in interface

Following the standard 12-factor architecture pattern for micro services, all logs are redirected by default to the standard output. By default, the output is formatted in an apache-like format. To enable the logs collection by the dedicated Cells service, you have to switch the "production" mode in order to output logs in JSON format. This format will allow further logs aggregation by external systems like ELK (Elastic Search, Logstash, Kibana).

Switching to 'production' mode can be done by two ways: 
* Setting the **PYDIO_LOGS_LEVEL** environment variable to "production": use `export PYDIO_LOGS_LEVEL=production` in the command line before starting cells
* Passing the `--log=production` parameter to the cells start command.

#### Browsing logs

Once activated, in the pydio cells interface you can find the logs under **Backend > Logs** :

[!image-popup:2_getting_started/server_logs.png]

You can read them as the following :
1. **Date** : the at time when the event happened
2. **I.P** : the ip if it happened elsewhere (could be a user etc...)
3. **User** : the username of whom it happened to
4. **Service** : the service that is handling it
5. **Message** : what happened with the service concerned (such as stopping, restarting, etc...)

*You can left click any error to have a detailed windows*

You can filter what you want to display by choosing the type of filter first then you fill the filed that are just on it's left (*Filter Logs*), to do so click on the **inverted triangle** then choose the type :

* **Full-text search** : you can filter the result by anything such as usernames, etc...

* **Pick one day** : pretty straightforward you will have a calendar displaying, allowing you to choose the day of the logs.

* **Time Period** : you can choose more than one day by putting **from** this day **to** that day.

#### Logs location on the server

When using Supervisorctl, logs are also written on-file on the server, they are located in `~/.config/pydio/cells/logs/cells.out`. They are rotated on a per-day basis.