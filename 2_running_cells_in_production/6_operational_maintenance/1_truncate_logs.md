With time, running Cells in production may end up generating an important amount of log files and other non-business data.
This page describes the locations you may have to watch for storage usage on your server, and eventually how you can automate cleaning/truncation of such data.

## System Logs

Assuming you are running in `--log=production` mode, all application logs will be stored both on file and inside an internal database.

### Files

Rotating files under `${CELLS_WORKING_DIR}/logs/`: these are outputted in JSON for easy collection by external tools (Elastic/Logstash/Kibana stacks or equivalent).

These files are automatically rotated based on their size (10MB max per file) and copies are removed after 28 days.

### Internal DB

In order to be able to easily display them and search them in the user interface, logs are **also** collected by an internal service and stored inside an internal DB (Bleve).

This on-file database can be found under `$CELLS_SERVICE_DIR/pydio.grpc.log/syslog.bleve`. This one is automatically rotated based on its size (200MB per DB), but it is not truncated automatically.

When the size of the DB reaches its maximum size, it is replicated for backup, thus in the folder you may see the following listing. DB folder with **the greatest index is always most recent**.

```SH
 $> ls $CELLS_SERVICE_DIR/pydio.grpc.log
 syslog.bleve
 syslog.bleve.001
 syslog.bleve.002
 syslog.bleve.003
 syslog.bleve.004
....
```

**[ED] Cleaning DB folders with Scheduler/Cells Flows**

As of Cells Enterprise v3, a ready-to-use job is inserted in the Scheduler (or Cells Flows if you have a license for it) that triggers automatically the removal of older DB folders. Job is disabled by default, but you can enable it and either run it manually or program a schedule for automated runs.

Job parameters allow setting the maximum size to keep, and an optional maximum size for Audit Logs databases as well (see below about Audit Logs).

**Cleaning DB folders with Command-Line**

For home users, logs purge can be applied using command line with the `clean logs` command. Its usage is described below:

```
Usage:
  ./cells admin clean logs [flags]

Flags:
  -s, --service string     Log service to truncate (use log by default) (default "log")
  -t, --threshold string   Size of logs to keep, specify in bytes (e.g 50MB, 1GB, ...)
```


**Cleaning DB folders manually**

You can safely remove oldest folders. Restarting Cells will automatically rename remaining folders
consistently. For example:

```SH
# Delete oldest foldesr
$> cd $CELLS_SERVICE_DIR/pydio.grpc.log
$> rm -rf syslog.bleve syslog.bleve.001 syslog.bleve.002
# List content
$> ls
syslog.bleve.003
syslog.bleve.004
# ==> RESTART CELLS and check content
$> ls
syslog.bleve
syslog.bleve.001
```


## [ED] Audit Logs

In Cells Enterprise, Audit Logs are handled in the same way as system logs. You can use methods described in the previous section, just replacing the `pydio.grpc.log` service by `pydio.grpc.audit`.

For example:

```SH
 $> ls $CELLS_SERVICE_DIR/pydio.grpc.audit
 auditlog.bleve
 auditlog.bleve.001
 auditlog.bleve.002
```

Or

```SH
# Truncate at 100MB
$> ./cells-enterprise admin clean logs --service audit --threshold=100MB
```

Beware that you should export your audit log before truncating them, as they contain sensitive information!