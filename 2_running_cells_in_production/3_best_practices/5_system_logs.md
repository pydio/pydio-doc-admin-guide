Following the standard 12-factor architecture pattern for micro-services, all logs are redirected by default to the standard output. They are also output in rotating log files under `CELLS_WORKING_DIR/logs/pydio.log`.

### Production mode

By default, logs are formatted in an Apache-like format. To enable logs collection by external systems like ELK (Elastic Search, Logstash, Kibana), you have to switch to the "production" mode: log messages are then output in JSON format. When enabled, an embedded log collector will start storing and parsing them for easier search in the logs.

Switching to 'production' mode can be done by two ways:

- Passing the `--log=production` parameter to the Pydio Cells `start` command.
- Setting the **CELLS_LOGS_LEVEL** environment variable to "production":  you can use `export CELLS_LOGS_LEVEL=production` in the Command Line before starting Pydio Cells.

### Browsing the logs

Once production mode is activated, the logging service starts collecting and parsing logs and you can browse them in your admin dashboard under **Backend > Logs**:

[:image-popup:2_running_cells_in_production/server_logs.png]

You can read them as the following:

1. **Date**: the at time when the event happened  
2. **IP**: the ip if it happened elsewhere (could be a user etc...)  
3. **User** the username of whom it happened to  
4. **Service**: the service that is handling it  
5. **Message**: what happened with the service concerned (such as stopping, restarting, etc...)  

*You can left click on any error to popup a detailed windows*.

Search the logs by using the search fields in the top right corner. For advanced filtering, click on the **inverted triangle** to choose a filter type and enter your parameters in the field that is on its left.

Filter types are:

- **Full-text search** (selected by default): filter by anything such as usernames, etc...
- **Pick one day**: pretty straightforward, choose a date day to see log for this day.
- **Time Period** : you can choose more than one day by putting **from** this day **to** that day.
