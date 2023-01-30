If you are already running a Cells instance on a mono-node server, you may want to improve its availability and scalability by replicating it to a multi-node cluster. 

## Prerequisites

You must first make sure to upgrade Cells to at least version 4. 

We also assume that you have read the previous sections of this chapter:  

 - [Going Stateless](./going-stateless) provides an overview of our clustering concepts,  
 - [Configuring with URLS](./configuring-urls) explains how to point Cells configuration toward the correct underlying services,   
 - [External Services](./external-services) explains how to set up all required cluster third-party services that will allow easy replication of the Cells image.  

## Migrating on-file services data from Bolt/BleveDB to MongoDB

_If you are already running a Cells V4 (or later) version with MongoDB, you can skip this section._

The first dataset that must be made portable accross images is all the services-generated data that are stored on file-based DBs: parsed logs, audit data, activities, etc... This can be achieved by copying all these in a Mongo database and reconfiguring services to point to Mongo.

To do that, simply use the following command : 

`$> cells admin config db add`

When prompted, choose _'NoSQL'_ database type and enter the MongoDB connection string (hostname, port, database name, etc).

```
$> ./cells admin config db add    
✔ NoSQL
✔ mongodb
✔ Please enter MongoDB server address. Can be in the form user:pass@host:port/dbName?key=value: █ocalhost:27017/cells?maxPoolSize=20&w=majority
? Do you wish to use this storage for all services supporting MongoDB driver? [Y/n] █
```
At this point, select Y to modify configuration of all services to use Mongo instead of Bolt/Bleve. 
```
## Performing NoSQL Installation
✔ Starting installation now
Assigning Document DSN to pydio.grpc.jobs
Assigning Document DSN to pydio.grpc.jobs
Assigning Document DSN to pydio.grpc.activity
Assigning Document DSN to pydio.grpc.log
Assigning Document DSN to pydio.grpc.docstore
Assigning Document DSN to pydio.grpc.search
Assigning Document DSN to pydio.grpc.chat
Assigning Document DSN to pydio.grpc.mailer
Assigning Document DSN to pydio.grpc.versions
✔ Created main database
? Do you wish to run migration for all assigned services?? [y/N] █
```
Finally, choose y to **copy** all data from Bolts and Bleve databases to Mongo. This may take some time, especially if you have a lot of logs and activities. 
Once finished, your MongoDB is filled with data.

You can test this migration by starting Cells as usual with `./cells start`. Everything should work as expected. 

Still, the pydio.grpc.search engine is not migrated, and you have to manually trigger a full re-indexation of the search engine, with `./cells admin resync --service pydio.grpc.search`. After that operation is finished, try to search something in the search engine, you should find correct results.

## Migrating configuration data from json to ETCD+VAULT

The other dataset to migrate from local filesystem to the external storages are all config-related. For that, you will use `./cells admin config copy` that expects --from and --to parameters in the URL form. 

Run `CELLS_DISPLAY_HIDDEN_FLAGS=true ./cells start --help` to look at the default URLs used for `--config`, `--certs_store`, `--keyring` and `--vault`.

1 - Copy configs from JSON to ETCD:
```
$> ./cells admin config copy --from /home/pydio/.config/pydio/cells/pydio.json --to etcd://:2379/config
```
2 - Copy cells vault from JSON to ETCD:
```
$> ./cells admin config copy --from /home/pydio/.config/pydio/cells/pydio-vault.json --to etcd://:2379/vault
```
3 - Copy keyring from File to Vault:
```
$> ./cells admin config copy --from /home/pydio/.config/pydio/cells/cells-vault-key --to vault://:8200/kv/keyring
```

Warning, do not confuse cells vault (which is simply an encrypted config) with Hashicorp Vault (which is a secrets storage that we use to store the keyring).

## Starting Cells with the new parameters

Once all configurations are live in ETCD and Vault, you can now set up Cells runtime using flags or environment variables: 
```
$> export CELLS_CONFIG=etcd://:2379/config
$> export CELLS_VAULT=etcd://:2379/vault
$> export CELLS_KEYRING=vault://:8200/kv
$> export CELLS_CERTS_STORE=vault://:8200/caddycerts
$> export CELLS_CACHE=redis://:6379/cells
$> export CELLS_BROKER=nats://:4222/cells

# And finally:
$> ./cells start
```
