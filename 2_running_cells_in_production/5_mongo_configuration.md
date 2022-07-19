## Why Mongo?

Historically Cells has been using BoltDB and BleveSearch and we like it. They are pure GO key/value stores and indexers and it allows **Cells to provide full indexing functionality without external dependencies**. By default, the search engine, activity stream or logs use these JSON document shops to provide rich, out-of-the-box functionality. 

But these stores are disk and memory intensive, and while they are suitable for small and medium-sized deployments, they create bottlenecks for large deployments. **MongoDB provides a feature-rich, scalable and indexed JSON document store**, perfectly suited to store the same kind of data and to scale horizontally.

A couple of notes: 

- **File-based storage is still a very good option for small/medium instances**, avoiding the need to manage another dependency.
- **You can migrate from one to another afterward** (see below).
- **Mongo does not replace MySQL storage**, a MySQL/Maria DB is always required to run Cells.

## When to use?

Depending on your target deployment size, you may switch from the default on-file storage used by specific services (search engine, activity feeds, logs, amongst others) to a Mongo DB document store.  So this is a good idea if:

- You foresee a high load on the platform:  number of users and/or a high number of files managed every day.
- You plan to deploy Cells in a distributed environment (cluster) to provide high availability or horizontal scaling.

The following services can either use on-file storage (BoltDB or BleveSearch) or a Mongo storage: 

|Service Name| Description                                    |
|------------|------------------------------------------------|
|pydio.grpc.versions| documents revisions informations               |
|pydio.grpc.jobs| scheduler flows definitions, statuses and logs |
|pydio.grpc.chat| chat rooms and chat messages                   |
|pydio.grpc.docstore| generic structured document store (JSON)       |
|pydio.grpc.log| application logs                               |
|pydio.grpc.mailer| sent and pending emails                        |
|pydio.grpc.search| search engine                                  |
|pydio.grpc.activity| all activity feeds                             |
|[Ent] pydio.grpc.audit| audit logs                                     |
|[Ent] pydio.grpc.reports| audit reports|                                  
|[Ent] pydio.grpc.ipban| ipban definitions and caches|                   

## Mongo Connection

Mongo connection requires information as described below 

| Parameter                    | Description                                                                          | Defaults                  |
|------------------------------|--------------------------------------------------------------------------------------|---------------------------|
| Host/Port                    | Address of the server hosting the Mongo DB                                           | localhost:27317           |
| Credentials                  | Optional credentials to connect, along with an authentication DB name                | [none]                    |
| Database Name                | Name of the mongo database                                                           | cells                     |
| Connection String Parameters | Additional connection parameters passed via Mongo connection string query parameters | maxPoolSize=20&w=majority |


## Configuring Mongo at startup

The best option is to directly setup Mongo DB connection when running `./cells configure`. Both command-line and web installer prompt for an optional Mongo connection. In that case, Mongo will be directly used by all services supporting this storage from day one.

## Migrating Existing Instance 

If you are upgrading a pre-v4 instance or deliberately started with on-file databases, you may wish to switch to MongoDB afterward. Cells provides a command-line to achieve that. 

Make sure to shutdown Cells, then use the following command: 
```
./cells admin config db add
```

- Pick the "NoSQL" db type and fill the form with the Mongo DB connection information.
- Accept prompt for using this connection as default document DSN
- Accept prompt to perform data migration from existing bolt/bleve files to mongo. This can take some time.

Now restart Cells. You should see "Successfully pinged and connected to MongoDB" in the logs. As search engine data are not migrated, you have to relaunch indexation on the pydio.grpc.search service using: 

```
cells admin resync --service=pydio.grpc.search --path=/
```
