
## Activities model

In Pydio Cells, activities are designed as a generic concept following social network feeds model : an Object can receive activities from external events in its "inbox" and has its own self-triggered events exposed in its "outbox". This can model be compared to the social network feeds represented by a user "story" (outbox) and his/her wall ("inbox'). 

In Cells it is applied in different contexts:  

**Users "inboxes"** represent the notifications received when watching a file or folder for modifications.

[:image:2_running_cells_in_production/users-notifications.png]

**Nodes (files/folders) "outboxes"** registers all activities of a given node and all its children.

[:image:2_running_cells_in_production/folders-activities.png]

These activities are stored in the Activity Stream 2.0 format inside a onfile database. By default, these activities are kept forever, leading the on-file Bolt Database to grow, and eventually consume a lot of RAM. These activites are **not** used for Audit logs, and thus can be safely purged after a while.

## [Ent] Purging Users Notifications with Scheduler/Cells Flows

Starting with Cells v3, a default job is inserted at startup for purging activities based on specific parameters. It is disabled by default, you can enable it and either run manually or setup a scheduler for automated run.

[:image:2_running_cells_in_production/housekeeping-jobs.png]

"Purge Users Notifications" job has the following parameters:

| Parameters  | Description                                                                                       | Default |
| ----------- | ------------------------------------------------------------------------------------------------- | ------- |
| KeepAtLeast | Minimum number of activities to keep for each file or folder, starting from the most recent ones. | 1       |
| KeepLast    | Maximum number of activities to keep for each file or folder (removing the oldest ones            | 10      |
| UpdatedDays | Purge activities recorder before a specific date (today minus the number of days.                 | 30days  |

As explained above, this job applies to the "users' inboxes". After a purge, the activity database is entirely cloned and re-indexed for a better defragmentation.


### [Ent] Purging Files/Folders Activities using Scheduler/Cells Flows

Similarly to the "Purge Users Notifications", a "Purge Files Activities" job is available for automated run, with the same parameters. It is applied to the "nodes' outboxes". 

## Using Command-Line

For home users or for more options, it is possible to manually trigger an operation similar to the scheduler jobs by using the `clean activities` command.

```
Usage:
  ./cells admin clean activities [flags]

Flags:
  -a, --admin string     Provide login of the administrator user
  -b, --box string       Either inbox (notifications received) or outbox (user activity / file activity) (default "outbox")
      --clear            After DB compaction, remove original file, otherwise keep it as a backup
      --compact          Trigger DB compaction by copying boltDB into a new file (default true)
      --db string        Point directly to a DB file to perform the purge offline
  -h, --help             help for activities
      --max int          Clear by keeping a maximum number of records inside each box
      --min int          Keep at least N, 0 for clearing all records (default 1)
  -o, --owner string     Specific user or node ID, or all (default "*")
      --timeout string   Set a longer timeout if there are tons of activities to purge (duration) (default "6h")
  -t, --type string      Activity type, one of 'nodes' or 'users' (default "nodes")
      --updated string   Clear by keeping all records updated before a given date. Use golang duration, e.g. '3d' will keep all records updated 
```

You can find here the notions explained above : _type_ is one of "nodes" or "users", _box_ is one of "inbox" or "outbox". Other criteria for purging activities are similar to the jobs parameters.

An interesting flag is the `--db` allowing to directly point to a database file. This allows **applying the purge operation offline**. This must be done with Cells service stopped, and can be applied to the `{CELLS_WORKING_DIR}/services/pydio.grpc.activity/activities.db` file.