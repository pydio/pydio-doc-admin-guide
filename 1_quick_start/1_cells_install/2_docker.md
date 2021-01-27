[:image:logos-os/logo-docker.png]

The [Pydio Cells image for Docker](https://hub.docker.com/r/pydio/cells/) is designed to be used in a micro-service environment. It only contains what is strictly necessary to run your server.

## Run as stand-alone container

Pydio Cells only needs a [MySQL / MariaDB Database with the corresponding admin user](./v2/requirements).

You can launch a test instance with:

```sh
docker run -d --network=host pydio/cells
```

Finalise the configuration at [https://localhost:8080](https://localhost:8080) with your MySQL/MariaDB credentials and you are good to go.

If your server has a public IP address and has no restriction on this port (firewall...), your instance is also directly exposed at [https://<YOUR-SERVER-IP-ADDRESS>:8080](.).

Before you go live, you probably want to configure persistent data in a docker volume.  
Assuming you also have a registered domain name (FQDN) for your server, you could go with:

```sh
FQDN=<Put Your FQDN here>
docker run -d  -v /home/user/cells_dir:/var/cells -e CELLS_BIND=:443 -e CELLS_EXTERNAL=https://$FQDN:8080 --network=host pydio/cells
```

Where:

- `-d`: run in the background
- `-v /home/user/cells_dir:/var/cells`: mount a local folder as Cells working directory
- `-e CELLS_BIND=:443`: use standard reserved port for HTTPS (must be unused, typically by a webserver)
- `-e CELLS_EXTERNAL=https://$FQDN`: (optional) explicitely declare your domain
- `--network=host`: directly use the host network, to easily connect to the DB

## Run with docker-compose

Below is a vanilla configuration to run Pydio Cells with `docker-compose`:

```yaml
version: '3.7'
services:

  cells:
    image: pydio/cells:latest
    restart: unless-stopped
    ports: ["8080:8080"]
    environment:
      - CELLS_LOG_LEVEL=production
    volumes:
      - data:/var/cells/data
      - cellsdir:/var/cells

  mysql:
    image: mysql:5.7
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: P@ssw0rd
      MYSQL_DATABASE: cells
      MYSQL_USER: pydio
      MYSQL_PASSWORD: P@ssw0rd
    command: [mysqld, --character-set-server=utf8mb4, --collation-server=utf8mb4_unicode_ci]
    volumes:
      - mysqldir:/var/lib/mysql

volumes:
    data: {}
    cellsdir: {}
    mysqldir: {}
```

## Go further

### Commands

When you `run` the image without command, it executes:

- `cells configure`, if no installation is found
- `cells start`, otherwise

If you add a command, it is executed instead, e.g:

```sh
docker run pydio/cells cells version
# or to log in a running container with id 5fe... 
docker exec 5fe /bin/sh
```

### Data layout

If you want to use named volumes, here is an overview of the important files :

- `/var/cells`: main working dir
- `/var/cells/pydio.json`: main configuration file
- `/var/cells/data`: data
- `/var/cells/logs`: logs
- `/var/cells/certs`: certificate management
- `/var/cells/services`: services information

### Environment variables

As previously seen, when launching the image, the `start` (or `configure`, on 1st launch) command is called: it means that all flags are settable with their associated ENV var, using upper case and CELLS_ prefix.

Below is an extract of relevant ENV variables that you can pass to the container.

| Name           | Value                   | Default          |
| -------------- | ----------------------- |  ---------------- |
| CELLS_BIND     | host:port               |  0.0.0.0:8080     |
| CELLS_EXTERNAL | http(s)://url-to-access  | (none) |
| CELLS_NO_TLS   | 1 = noTLS, 0 = TLS      | 0           |
| CELLS_WORKING_DIR   | path in the container | /var/cells           |
| CELLS_LOG_LEVEL   | a valid log level | info           |


### TRASH

#### Persistent data

All default configuration and data (`/var/cells` on the container) is saved in an unnamed volume.


Note: If you [add new datasources](./managing-datasources) and want to persist the data, ensure that their location is also mounted in a volume.

#### Environment variable

- `CELLS_NO_TLS`: uses tls or not (default to 0 => uses tls)
- `CELLS_BIND` : address where the application http server is bound to. It MUST contain a server name and a port.
- `CELLS_EXTERNAL` : url the end user will use to connect to the application.

Let's say:

- You have a server with an internet facing IP and a corresponding DNS A entry that points toward `files.example.com`
- You want to run the application in self signed mode on port 8080 (remember the default value of CELLS_NO_TLS is disabled)

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

### Public access

#### HTTPS

We recommend you [run behind a proxy](https://pydio.com/en/docs/kb/devops) to encrypt the content you want to publish over the internet.

The [nginx-proxy](https://github.com/jwilder/nginx-proxy) and [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) containers can also be used to setup your proxy.

### Cells Sync

The Cells Sync Desktop Application will require an additional port.

- First read this, [Setup Cells Server for Cells Sync](/en/docs/kb/client-applications/setup-cells-server-cellssync)
- Make sure to start a container with this env set `PYDIO_GRPC_EXTERNAL`
- Expose the port that you previously set with `PYDIO_GRPC_EXTERNAL`

Example:

Assuming that port **33060** is the port chosen for gRPC, the command should have those two additional parameters,

- `-e PYDIO_GRPC_EXTERNAL=33060` (sets the env variable)
- `-p 33060:33060` (exposes the port)

The entire command should look like this:

```sh
docker run -d -e CELLS_EXTERNAL=192.168.0.172:8080 -e CELLS_BIND=192.168.0.172:8080 -e PYDIO_GRPC_EXTERNAL=33060 -p 33060:33060 -p 8080:8080 pydio/cells
```

### Troubleshooting

#### Check that after a server or container reboot that the address is the same

If you ever restart your server and or your containers docker might attribute you another address for your containers.

For instance docker usually has containers addresses looking like this `172.17.0.5` and therefore in Cells the default datasources or the ones that you will be creating on the filesystem will all be using this peer address by default.

You might stumble upon this issue, if that's the case you need to access the `pydio.json` file and change the old occurrences of the address with the newest one.
