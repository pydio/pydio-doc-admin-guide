This section shows how to run Cells in a multi-node setup using out-of-the-box docker compose file.

## Docker Compose file

Grab the compose file located at [https://github.com/pydio/cells/blob/main/tools/docker/compose/ha/docker-compose.yml](https://github.com/pydio/cells/blob/main/tools/docker/compose/ha/docker-compose.yml). 

It defines the following services: 

 - `mysql`, `mongo`, `redis` for relational, json and KV stores
 - `etcd`, `nats`, `vault` for configs, message broker and secret store.
 - `minio` for exposing a file system as an S3 object storage
 - `cells1`, `cells2`, `cells3` multiple replicas of cells services
 - `caddy` as a frontend reverse proxy load-balancing on cells services

## Setup and Deployment

Simple docker-compose deployment to experiment with Cells v4 Clustering model.
It uses `pydio/cells:unstable` docker image, use whatever image by editing the docker-compose.yml file.

### Preparing and starting dependencies

HA deployments relies on external dependencies to make Cells image fully stateless.
This sample creates the following images : MySQL, MongoDB, NATS.io, ETCD, Hashicorp Vault and Redis.

Some preparatory steps are required : create Minio buckets for shared storage, and create a specific key/value store in Vault. 

```sh
cd <this folder>
# start all third-party services
docker-compose up -d mysql mongo nats etcd vault redis minio caddy

# create buckets in minio 
docker-compose up createbuckets

# Create a dedicated kvstore for certificates in Vault (configured in DEV mode with a preset VAULT_TOKEN, this should not be the case in production)
docker-compose exec -e VAULT_ADDR=http://localhost:8200 -e VAULT_TOKEN=secret_vault_token vault vault secrets enable -version=2 -path=caddycerts kv
```

### Caddy LoadBalancer

Caddy load balancer is configured in self-signed mode with a `caddy` domain name.
This requires adding `localhost => caddy` domain name to your local /etc/hosts file.

Once started, the proxy will monitor cells instances on /pprofs endpoint to automatically enable/disable upstreams.

### Running Cells Nodes

At first run, start a unique node to let it perform the automatic installation. This step is reading its values from the `conf/install-conf.yml` file, you may **change the frontendadmin / frontendpassword values to your own**, otherwise default credentials are admin/admin.

Monitor the logs to check that installation is complete or simply open https://caddy:8585/ to check that you can log in with admin credentials.

```sh
docker-compose up -d cells1; docker-compose logs -f cells1
```

Now you can spin more cells nodes:
```sh
# Once install is finished, start other nodes 
docker-compose up -d cells2 cells3; docker-compose logs -f cells2 cells3
```

At next run, you can start all three nodes at once.

Access https://caddy:8585/ to access Cells. Enjoy!

### Stopping cluster

```sh
# To clean everything
docker-compose down -v --remove-orphans
```