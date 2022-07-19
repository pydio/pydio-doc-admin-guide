Cells configuration for all storages is done using an "scheme-based" pattern, that fully describes a given driver used by Cells.

## Flags and URLs

Let's have a look at some of the available Flags advertised by the command (_output below is filtered_): 

```
% CELLS_DISPLAY_HIDDEN_FLAGS=true ./cells start --help

Flags:
      --broker string                Pub/sub service URL for events broadcast. Supported schemes: grpc|mem|nats|rabbit (default "mem://")
      --cache string                 Sharded Cache (default "bigcache://")
      --discovery string             Combine registry, config and pub/sub discovery service (default "mem://")
      --registry string              Registry URL used to manage services. Supported schemes: etcd|file|grpc|mem (default "mem://?cache=shared")
      --shortcache string            Short cache (default "pm://")

Global Flags:
      --certs_store string   Certificates Store URL. Can be switched to vault://host:port/secretPath (default "file:///Users/charles/Library/Application Support/Pydio/cells/certs")
      --config string        Configuration storage URL. Supported schemes: etcd|file|grpc|mem|vault|vaults (default "file:///Users/charles/Library/Application Support/Pydio/cells/pydio.json")
      --keyring string       Keyring URL. Can be switched to vault://host:port/secretPath?key=storeKey (default "file:///Users/charles/Library/Application Support/Pydio/cells/cells-vault-key?keyring=true")
      --vault string         Vault location, automatically detected from config url, unless an URL is provided (same schemes as config) (default "detect")

```

As you can see, most default values use a "mem://" (a.k.a memory) or "file://" scheme. This shows that data attached to these store will be maintained in memory or on-file. If we want one of these store to another technology, we can use one of the 'supported scheme' to pass a connection string and additional informations required by Cells with a different URL. For example, assuming an ETCD cluster is running and maintained aside in the cluster, we can easily start Cells with 
```
% ./cells start --registry=etcd://:2379
```

Or as all our Flags can be configured by their corresponding CELLS_[FLAG_NAME_UPPER_CASE] environment variable, we could use : 
```
% export CELLS_REGISTRY=etcd://:2379
% ./cells start
```

## Supported Schemes

We will explain below the supported schemes for each flag

### Registry

The registry maintains a list of all Cells services (started or not) available in the cluster.

Flag `--registry` or environment `CELLS_REGISTRY`

| Scheme | Example               | Comment                                                                                                                                                         |
|--------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mem    | mem://?cache=shared   | This is the default. It provides an in-memory registry that is exposed via gRPC by the pydio.grpc.registry service                                              |
| etcd   | etcd://:2379/[prefix] | Instead storing registry state in-memory, store it in an ETCD K/V store. This should be the default in Cluster mode.                                            |
| grpc   | grpc://:8002          | This is the default for fork processes (wether the main process is using mem:// or etcd://). Connects to a running pydio.grpc.registry locally on the 8002 port |


### Broker

The broker provides Publish/Subscribe pattern for broadcasting events.

Flag `--broker` or environment `CELLS_BROKER`

| Scheme | Example               | Comment                                                                                                                                                               |
|--------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mem    | mem://                | This is the default. It uses google cloud golang pub/sub library under the hood. It exposes itself as gRPC vie the pydio.grpc.broker service                          |
| nats   | nats://:4222/[prefix] | Connect to a Nats.io cluster for messaging. This should be the default in Cluster mode.                                                                               |
| grpc   | grpc://:8002          | This is the default for fork processes (wether the main process is using mem:// or nats://). Connects to a running pydio.grpc.broker service locally on the 8002 port |

### Config, Vault, Certificates

A watchable key/value API that is common to config and vault flags.

Flag `--config` or environment `CELLS_CONFIG` : unencrypted configuration, as they can be found in the pydio.json file  
Flag `--vault` or environment `CELLS_VAULT` : specific configurations that must be encrypted using a master key (stored in the Keyring, see below).  
Flag `--certs_store` or environment `CELLS_CERTS_STORE` : Encrypted store for Caddy to store generated certificates. Uses a master key (stored in the Keyring, see below).  

| Scheme | Example                         | Comment                                                                                                                           |
|--------|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| file   | file:///{path/to/pydio.json}    | This is the default. Stores configurations as JSON inside a local file.                                                           |
| grpc   | grpc://:8002                    | This is the default for fork processes. Connects to a running pydio.grpc.config service locally on the 8002 port                  |
| etcd   | etcd://:2379/[prefix]           | Used in cluster mode writing/reading configuration from an ETCD cluster                                                           |
| vault  | vault://localhot:8200/[prefix]  | Used in cluster mode for encrypted values, points to a Hashicorp Vault key/value secret storage exposed on https://localhost:8200 |
| vaults | vaults://localhot:8200/[prefix] | Same as previous but with TLS connection                                                                                          |

### Discovery

The discovery flag is simply a shortcut for `--broker`, `--registry` and `--config` to shorten the fork processes command.  
If you look at the cells process, you will see all forks running with `--discovery grpc://:8002`, as the main process on a given machine always starts a set of grpc services to expose configs, broker and registry.

### Cache

Caching layer to heavy data requests. 

Flag `--cache` or environment `CELLS_CACHE`

| Scheme   | Example         | Comment                                                                          |
|----------|-----------------|----------------------------------------------------------------------------------|
| bigcache | bigcache://     | Uses BigCache Go library (in memory, with pre-allocation of memory segments).    |
| pm       | pm://           | Uses patrickmm/cache Go library (in memory, simple synchronised maps)            |
| redis    | redis://:6379   | Connects to a Redis cluster for caching. Should be the default for Cluster mode. |

### Keyring

As explained above, Vault and Certificates Store require a master key for encrypting data. This one is stored in the keyring.

Flag `--keyring` or environment `CELLS_KEYRING`

| Scheme | Example                          | Comment                                      |
|--------|----------------------------------|----------------------------------------------|
| file   | file://{path-to-cells_vault_key} | Stores generated key on-file                 |
| vault  | vault://localhost:8200/kv        | Stores keyring data inside HashiCorp vault   |
