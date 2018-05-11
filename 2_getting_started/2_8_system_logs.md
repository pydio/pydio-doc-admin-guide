In this chapter we are going to have a close look at the logs and where they are stored and what is stored in them.

### System Logs

With pydio cells you have a logging system allowing you to always be informed about what's happening. The logs are storing every event happening on pydio cells such as a user creation, access, errors and more.

#### Pydio cells interface logs

*To enable the logs you will have to get this environment variable to this value
`PYDIO_LOGS_LEVEL=production`*
you can use the following command `export PYDIO_LOGS_LEVEL=production`.

You have for instance in the pydio cells interface the **server logs** you can find them in **Backend > Logs** :

![server logs](/images/2_getting_started/server_logs.png)

You can read them as the following :
1. **Date** : the at time when the event happened
2. **I.P** : the ip if it happened elsewhere (could be a user etc...)
3. **User** : the username of whom it happened to
4. **Service** : the service that is handling it
5. **Message** : what happened with the service concerned (such as stopping, restarting, etc...)

*You can left click any error to have a detailed windows*


you can filter what you want to display by choosing the type of filter first then you fill the filed that are just on it's left (*Filter Logs*), to do so click on the **inverted triangle** then choose the type :

* **Full-text search** : you can filter the result by anything such as usernames, etc...

* **Pick one day** : pretty straightforward you will have a calendar displaying, allowing you to choose the day of the logs.

* **Time Period** : you can choose more than one day by putting **from** this day **to** that day.

#### Logs located on your webserver

You can also look at the logs from your server, they are located in `~/.config/pydio/cells/logs`.
