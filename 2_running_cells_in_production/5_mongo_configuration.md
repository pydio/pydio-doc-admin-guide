## Why Mongo?

Historically we've been using BoltDB and BleveSearch and we like it. They are pure GO key/value stores and indexers and it allows Cells to provide full indexing functionality without external dependencies. By default, the search engine, activity stream or logs use these JSON document shops to provide rich, out-of-the-box functionality. But these stores are disk and memory intensive, and while they are suitable for small and medium-sized deployments, they create bottlenecks for large deployments.

We therefore looked at alternatives for implementing new 'drivers' for the data abstraction layer of each of these services, and chose MongoDB as a feature-rich, scalable and indexed JSON document store. All services using BoltDB/Bleve as storage now offer an alternative MongoDB implementation, a migration path from one another, and the ability to scale horizontally drastically.

File-based storage is still a very good option for small/medium instances, avoiding the need to manage another dependency, but the Cells installation steps will now offer to configure a MongoDB connection to avoid the need to migrate data in the long term. Note that Mongo does not replace MySQL storages, that DB is still required for Cells.

## When to use?

Depending on your target deployment size, you may switch from the default on-file storage used by specific services (search engine, activity feeds, logs, amongst others) to a Mongo DB document store.

This is a good idea if:

- You foresee a high load on the platform:  number of users and/or a high number of files managed every day.
- You plan to deploy Cells in a distributed environment (cluster) to provide high availability or horizontal scaling.

The following services can be configured to either use on-file storage (BoltDB or BleveSearch) or Mongo storage : 

- pydio.grpc.versions
- pydio.grpc.jobs
- pydio.grpc.jobs
- pydio.grpc.chat
- pydio.grpc.docstore
- pydio.grpc.log
- pydio.grpc.mailer
- pydio.grpc.search
- pydio.grpc.activity
- [Ent] pydio.grpc.audit
- [Ent] pydio.grpc.reports
- [Ent] pydio.grpc.ipban


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
