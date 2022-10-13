Once your environment is ready and all third-party dependencies are correctly running, you are now ready to start one or more 
replicas of Cells nodes.

## Starting Cells with proper parameters

As explained in Configuring with URLs, the key here will be to pass all the required flags (or their ENV counter-part) to the `cells configure`/`cells start` commands.

Below is a list of ENVIRONMENT variables that will redefine internal defaults to use external services:

    - CELLS_CONFIG=etcd://{ETCD_HOST}:{ETCD_PORT}/config
    - CELLS_VAULT=etcd://{ETCD_HOST}:{ETCD_PORT}/vault
    - CELLS_REGISTRY=etcd://{ETCD_HOST}:{ETCD_PORT}/registry
    - CELLS_BROKER=nats://{NATS_HOST}:{NATS_PORT}
    - CELLS_KEYRING=vault://{VAULT_HOST}:{VAULT_PORT}/secret?key=master
    - CELLS_CERTS_STORE=vault://{VAULT_HOST}:{VAULT_PORT}/caddycerts
    - CELLS_CACHE=redis://{REDIS_HOST}:{REDIS_PORT}
    - VAULT_TOKEN={VAULT_ROOT_TOKEN}

Assuming that you have an internal DNS resolving services by their names and that you are using default ports for all, this could look like : 

    - CELLS_CONFIG=etcd://etcd:2379/config
    - CELLS_VAULT=etcd://etcd:2379/vault
    - CELLS_REGISTRY=etcd://etcd:2379/registry
    - CELLS_BROKER=nats://nats:4222
    - CELLS_KEYRING=vault://vault:8200/secret?key=master
    - CELLS_CERTS_STORE=vault://vault:8200/caddycerts
    - CELLS_CACHE=redis://redis:6379
    - VAULT_TOKEN=${VAULT_ROOT_TOKEN}

Or, using command-line flags instead of Env, these could have been : 

    > export VAULT_TOKEN=${VAULT_ROOT_TOKEN} # that one still requires ENV 
    > cells start --config=etcd://etcd:2379 --registry=etcd://etcd:2379 --broker=nats://nats:4222 --keyring=vault://vault:8200/secret?key=master --certs_store=vault://vault:8200/caddycerts --cache=redis://redis:6379 

In the example above, we just specify the `config` URL: if no path is provided it is automatically switched to "URL/config", and if not CELLS_VAULT is provided, it is inferred from config URL to "URL/vault". 


### A. Beware of --vault vs --keyring.

Beware that the `CELLS_VAULT` flag is **not** pointing to the Hashicorp Vault service (which is used in the `CELLS_KEYRING`) but it points to a configuration container similar to `CELLS_CONFIG`, where values are encrypted. The master key used for this encryption is itself stored in... the Keyring!

### B. Hashicorp Vault initialization

As you can see in the commands above, Vault is used for both the master keyring and for storing Cells embedded proxy (Caddy) generated-certificates. You must make sure that both KV storages "secret" and "caddycerts" are createad before starting. On a basic Vault install, the "secret" does exist by default, but you must create "caddycerts" with the following command: 

    > export VAULT_TOKEN=${VAULT_ROOT_TOKEN}
    > vault secrets enable -version=2 -path=caddycerts kv

The ${VAULT_ROOT_TOKEN} should be a safe token that is injected in your image in one way or another. 

### C. Note about MongoDB

Mongo DB support is not enabled via a start flag, but it is configured at first deployment. An existing Cells instance can also be migrated to support Mongo as [described in the dedicated page](./configuring-mongo-storage).

## First Run / Installation

When running Cells for the first time, you have to go through the `cells configure` process to set up some core configurations. In a cluster, automated approach, you must find a way to populate this configuration with your preset values. 

### Running "cells configure"

One simple option is to simply run `cells configure` on the very first node you are starting, using the same flags/ENV as define above (to make sure that the config is written in ETCD).

### Using Docker

If you are using Docker, our official images provide an internal bootstrap mechanism that tries to detect wether a configuration is properly set, and if not automatically use `cells configure` instead of `cells start`. By passing a reference to a YAML/JSON installation file, you can let the first start ingest this file and automatically restart the container in `start` mode.