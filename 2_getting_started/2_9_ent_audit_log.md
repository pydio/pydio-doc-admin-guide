In the **Enterprise Edition**, an audit log gathers all non-technical events to help focus on business logic when analysing logs for compliance or when tracing events after a security breach.

Audit Logs are accessible under the section **Dashboard** inside the **Activity** tab.

Logs are grouped using a unique Request-Id as they may be produced by a unique action going through various micro-services.

When using Supervisorctl (or systemd) service, audit logs are also available in the `~/.config/pydio/cells/logs/audit.log` file (rotated every day).

[:image-popup:2_getting_started/audit_log_access.png]

## Exporting logs

For an easier analysis, audit logs can be exported in XLSX or CSV format.  
To do so, you can use the button in the top-right of the audit logs screen. Please note that you must first filter the logs (either by searching or by restricting to a given period) to avoid exporting the whole base of logs at once.