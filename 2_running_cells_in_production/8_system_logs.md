In this section, we have a closer look at the logs, where they are stored and what is stored.

### System Logs

With Pydio Cells you have a logging system allowing you to always be informed about what is happening. Logs are recording every event such as user creations, accesses, errors and more.

#### Enabling logs in interface

Following the standard 12-factor architecture pattern for micro services, all logs are redirected by default to the standard output. By default, the output is formatted in an apache-like format. To enable the logs collection by the dedicated Pydio Cells service, you have to switch to the "production" mode: log messages are then output in JSON format. This format allows later log aggregation by external systems like ELK (Elastic Search, Logstash, Kibana).

Switching to 'production' mode can be done by two ways:

- Setting the **PYDIO_LOGS_LEVEL** environment variable to "production":  you can use `export PYDIO_LOGS_LEVEL=production` in the Command Line before starting Pydio Cells.
- Passing the `--log=production` parameter to the Pydio Cells `start` command.

#### Browsing the logs

Once activated, you can find the logs in your admin dashboard under **Backend > Logs**:

[:image-popup:2_running_cells_in_production/server_logs.png]

You can read them as the following:

1. **Date**: the at time when the event happened  
2. **I.P**: the ip if it happened elsewhere (could be a user etc...)  
3. **User** the username of whom it happened to  
4. **Service**: the service that is handling it  
5. **Message**: what happened with the service concerned (such as stopping, restarting, etc...)  

*You can left click on any error to popup a detailed windows*.

You can directly search the logs by using the search field in the top right corner.

To do advanced filtering, click on the **inverted triangle** to choose a filter type and enter the your parameters in the field that is on its left.

Filter types are:

- **Full-text search** (selected by default): filter by anything such as usernames, etc...
- **Pick one day**: pretty straightforward, choose a date day to see log for this day.
- **Time Period** : you can choose more than one day by putting **from** this day **to** that day.

#### Logs location on the server

When running as a service, logs are also written on-file on the server, they are located in `~/.config/pydio/cells/logs/cells.log`. They are rotated on a per-day basis.
