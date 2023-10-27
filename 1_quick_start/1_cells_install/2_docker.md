
The [Pydio Cells image](https://hub.docker.com/r/pydio/cells/) is a container designed to be used in a Docker environment. It only contains what is strictly necessary to run your server.

## Run as stand-alone container

Pydio Cells needs a MySQL/MariaDB Database [with a privileged user](https://pydio.com/en/docs/cells/v4/requirements).

Launch a test instance with:

```sh
docker run -d --network=host pydio/cells
```

Enter your database credentials to finalise the configuration at [https://localhost:8080](https://localhost:8080) and you are good to go.

If your server has a public IP address and no restriction on the chosen port (firewall...), your instance is also directly exposed at `https://<YOUR-SERVER-IP-ADDRESS>:8080`.

Before you go live, you probably want to configure persistent data in a docker volume. Assuming you also have a registered domain name (FQDN) for your server, you could go with:

```sh
FQDN=<Put Your FQDN here>
docker run -d  -v /home/user/cells_dir:/var/cells -e CELLS_SITE_BIND=:443 -e CELLS_SITE_EXTERNAL=https://$FQDN --network=host pydio/cells
```

Where:

- `-d`: run in the background
- `-v /home/user/cells_dir:/var/cells`: mount a local folder as Cells working directory
- `-e CELLS_SITE_BIND=:443`: use standard reserved port for HTTPS (must be unused, typically by a webserver)
- `-e CELLS_SITE_EXTERNAL=https://$FQDN`: (optional) explicitely declare your domain
- `--network=host`: directly use the host network, to easily connect to the DB

## Run with Docker Compose

Below is a vanilla configuration to run Pydio Cells with `docker compose`:

```yaml
version: '3.7'
services:

  cells:
    image: pydio/cells:latest
    restart: unless-stopped
    ports: ["8080:8080"]
    volumes:
      - cellsdir:/var/cells
      - data:/var/cells/data

  mysql:
    image: mysql:8
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

**Note**: with this configuration file, in the web installer, use:

- Host Name: mysql
- DB User: pydio
- DB Password: P@ssw0rd

and let the other parameters unchanged to configure the DB connection. 

## Go further

### Commands

The default commands when `running` the image are:

- `cells configure` if no installation is found
- `cells start` otherwise

If you specify a command, it willd be executed instead, e.g:

```sh
docker run pydio/cells cells version
# or to log in a running container with id 5fe... 
docker exec 5fe /bin/sh
```

### Data layout

Below is a list of important files and data present in the volume:

- `/var/cells`: main working dir
- `/var/cells/pydio.json`: main configuration file
- `/var/cells/data`: data
- `/var/cells/logs`: logs
- `/var/cells/certs`: certificate management
- `/var/cells/services`: services information

### Environment variables

As previously seen, when launching the image, the `start` (or `configure` on 1st launch) command is called: it means that all flags are settable with their associated ENV var, using upper case and CELLS_ prefix.

Below is an extract of relevant ENV variables that you can pass to the container.

| Name                | Value                   | Default      |
|---------------------|-------------------------|--------------|
| CELLS_SITE_BIND     | host:port               | 0.0.0.0:8080 |
| CELLS_SITE_EXTERNAL | http(s)://url-to-access | (none)       |
| CELLS_SITE_NO_TLS   | 1 = noTLS, 0 = TLS      | 0            |
| CELLS_WORKING_DIR   | path in the container   | /var/cells   |
| CELLS_LOG           | a valid log level       | info         |

### More examples

We gather some [relevant sample configuration](https://github.com/pydio/cells/tree/main/tools/docker/compose) in our main code base. Find the example that suits you and fine tune your setup.

### Cells Sync

The Cells Sync Desktop Application might require an additional port, for example if you run behind a reverse proxy that performs TLS termination. In such case:

- First read this, [Setup Cells Server for Cells Sync](https://pydio.com/en/docs/kb/client-applications/setup-cells-server-cellssync)
- Make sure to start a container with this env set `CELLS_GRPC_EXTERNAL`
- Expose the port that you previously set with `CELLS_GRPC_EXTERNAL`

#### Example

Assuming that port **33060** is the port chosen for gRPC, the command should have those two additional parameters,

- `-e CELLS_GRPC_EXTERNAL=33060` (sets the env variable)
- `-p 33060:33060` (exposes the port)

The entire command should look like this:

```sh
docker run -d -e CELLS_SITE_EXTERNAL=192.168.0.172:8080 -e CELLS_SITE_BIND=192.168.0.172:8080 -e CELLS_GRPC_EXTERNAL=33060 -p 33060:33060 -p 8080:8080 pydio/cells
```
