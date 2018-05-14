In the **Cells Enterprise Edition**, an audit log will gather all non-technical events to help focus on business logic when analysing logs for compliance, or for example when tracing events after a security breach.

Audit Logs are accessible via the **Activity** tab in the Settings Dashboard. An interface similar to the System Logs allows browsing, searching and filtering of these logs. Clicking on a row will give details about the event. 

Logs are grouped using a unique Request-Id as they may be produced by a unique action going through various micro-services.

When using Supervisorctl service, audit logs are also available in the `~/.config/pydio/cells/logs/audit.log` file (rotated every day).

#### Exporting logs

For an easier analysis, audit logs can be exported in XLSX or CSV format. To do that, you can use the button in the top-right of the audit logs screen. Please note that you must first filter the logs (either by searching or by restricting to a given period) to avoid exporting the whole base of logs at once.
