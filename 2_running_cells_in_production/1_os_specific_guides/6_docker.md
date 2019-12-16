_This guide describes the steps required to have the Pydio Cells Docker container running._

[:image:2_running_cells_in_production/logos-os/logo-docker.png]

### How to use the Pydio Cells docker image

The [Pydio cells image](https://hub.docker.com/r/pydio/cells/) is designed to be used in a micro-service environment. It only contains what is strictly necessary to run the Pydio Cells binary and nothing more.

In order to have a fully working Pydio Cells environment, you need to run a database (MySQL or MariaDB) on the same network. A basic setup is described on this page.

| env variable        | value                                | example                       |
| ------------------- | ------------------------------------ | ----------------------------- |
| CELLS_BIND          | host:port                            | localhost:80                  |
| CELLS_EXTERNAL      | http(s)://url-to-access              | http://localhost              |
| CELLS_NO_TLS        | 1 = noTLS, 0 = TLS                   | 0 or 1                        |
| PYDIO_GRPC_EXTERNAL | any port (must also expose the port) | this env variable is optional |


#### Run the container

```sh
docker run -d --network=host pydio/cell
```

You can now access the Pydio Cells installer at [https://localhost](https://localhost). A complete example can be found in the docker-compose section of this guide.

If you want to access your container from outside you must then change at least the `CELLS_EXTERNAL` and also (it is recommended) to have persisting data you need to have a volume (you can sort it with multiple volumes if you wish, but for our example we are going to store everything into a single folder).

Here's an example of a command that runs a cells container with persistent data and external access:

```sh
docker run -d -e CELLS_EXTERNAL=192.168.0.172:8080 -e CELLS_BIND=192.168.0.172:8080 -p 8080:8080 -v /home/cells/volume/:/var/cells pydio/cells
```

* **-e** *CELLS_EXTERNAL* is required to give it external access
* **-e** *CELLS_BIND* can be, the server running the container address or localhost.
* **-v** */home/cells/volume/:/var/cells*, basically I have a folder on my server located here `/home/cells/volume/` and where I want to store the whole Cells working directory.

_This was only an example on how you can run a Cells container, you can find below all of the enivronment variables, data configurations for cells, docker-compose examples and more_.

#### External database

Pydio Cells requires a MySQL or MariaDB database. It is recommended to use a separate database and a dedicated user with access to that database. A complete example can be found in the docker-compose section of this guide.

#### Persistent data

All default configuration and data (`/var/cells` on the container) is saved in an unnamed volume.

If you want to use named volumes, here is an overview of the important files :

* `/var/cells/pydio.json`: main configuration file
* `/var/cells/data`: data
* `/var/cells/logs`: logs
* `/var/cells/certs`: certificate management
* `/var/cells/services`: services information

Note: If you [add new datasources](./managing-datasources) and want to persist the data, ensure that their location is also mounted in a volume.

#### Environment variable

* `CELLS_NO_TLS`: uses tls or not (default to 0 => uses tls)
* `CELLS_BIND` : address where the application http server is bound to. It MUST contain a server name and a port.
* `CELLS_EXTERNAL` : url the end user will use to connect to the application.

Let's say:

* You have a server with an internet facing IP and a corresponding DNS A entry that points toward `files.example.com`
* You want to run the application in self signed mode on port 8080 (remember the default value of CELLS_NO_TLS is disabled)

### Example setup with docker compose

```yaml
version: '3'
services:

    cells:
        image: pydio/cells:latest
        restart: always
        ports: ["8080:8080"]
        environment:
            - CELLS_BIND=files.example.com:8080
            - CELLS_EXTERNAL=https://files.example.com:8080
       volumes:
            - "cellsdir:/var/cells"
            - "data:/var/cells/data"

    # MySQL image with a default database cells and a dedicated user pydio
    mysql:
         image: mysql:5.7
         restart: always
         environment:
             MYSQL_ROOT_PASSWORD: P@ssw0rd
             MYSQL_DATABASE: cells
             MYSQL_USER: pydio
             MYSQL_PASSWORD: P@ssw0rd
         command: [mysqld, --character-set-server=utf8mb4, --collation-server=utf8mb4_unicode_ci]
         volumes:
             - "mysqldir:/var/lib/mysql"

volumes:
    data: {}
    cellsdir: {}
    mysqldir: {}
```

### Sync desktop client

For users of the Sync desktop client you must configure and expose the gRPC port.

- Set this environment variable when starting your container with a port of your choosing, `PYDIO_GRPC_EXTERNAL=<port>`.
- then make sure to expose the port.

Example: 

Assuming that port **33060** is the port chosen for gRPC, the command should have those two additional parameters,

- `-e PYDIO_GRPC_EXTERNAL=33060` (sets the env variable)
- `-p 33060:33060` (exposes the port)

The entire command should look like this:

```
docker run -d -e CELLS_EXTERNAL=192.168.0.172:8080 -e CELLS_BIND=192.168.0.172:8080 -e PYDIO_GRPC_EXTERNAL=33060 -p 33060:33060 -p 8080:8080 pydio/cells
```

### Public access

#### HTTPS

We recommend you [run behind a proxy](https://pydio.com/en/docs/kb/devops) to encrypt the content you want to publish over the internet.

The [nginx-proxy](https://github.com/jwilder/nginx-proxy) and [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) containers can also be used to setup your proxy.

### Cells Sync

The Cells Sync Desktop Application will require an additional port.

* First read this, [Setup Cells Server for Cells Sync](/en/docs/kb/client-applications/setup-cells-server-cellssync)
* Make sure to start a container with this env set `PYDIO_GRPC_EXTERNAL`
* Expose the port that you previously set with `PYDIO_GRPC_EXTERNAL`

### Troubleshooting

#### Check that after a server or container reboot that the address is the same

If you ever restart your server and or your containers docker might attribute you another address for your containers.

For instance docker usually has containers addresses looking like this `172.17.0.5` and therefore in Cells the default datasources or the ones that you will be creating on the filesystem will all be using this peer address by default.

You might stumble upon this issue, if that's the case you need to access the `pydio.json` file and change the old occurrences of the address with the newest one.
