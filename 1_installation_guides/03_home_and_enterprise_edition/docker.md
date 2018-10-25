_This guide describes the steps required to have the Pydio Cells Docker container running._

[:image-popup:1_installation_guides/logos-os/logo-docker.png]

### How to use the Pydio Cells docker image

The [Pydio cells image](https://hub.docker.com/r/pydio/cells/) is designed to be used in a micro-service environment. It only contains what is strictly necessary to run the Pydio Cells binary and nothing more.

In order to have a fully working Pydio Cells environment, you need to run a database (MySQL or MariaDB) on the same network. A basic setup is described on this page.

#### Run the container

```sh
docker run -d -p 8080:8080 pydio/cells
```

You can now access the Pydio Cells installer at [https://localhost:8080](https://localhost:8080). A complete example can be found in the docker-compose section of this guide.

#### External database

Pydio Cells requires a MySQL or MariaDB database. It is recommended to use a separate database and a dedicated user with access to that database. A complete example can be found in the docker-compose section of this guide.

_Be advised if you pull the latest mysql image it will be MySQL 8 which has a new authentication method and which is for the moment not compatible (it's on our roadmap)_.
_We recommand that you pull MySQL version 5.7 for the time being_

#### Persistent data

All default configuration and data (`/root/.config` on the container) is saved in an unnamed volume.

If you want to use named volumes, here is an overview of the important files :

- `/root/.config/pydio/cells/pydio.json` : main configuration file
- `/root/.config/pydio/cells/data` : data
- `/root/.config/pydio/cells/logs` : logs
- `/root/.config/pydio/cells/services` : services information
- `/root/.config/pydio/cells/static` : static frontend files

Note: If you [add new datasources](https://pydio.com/fr/docs/cells/v1/managing-datasources) and want to persist the data, ensure that their location is also mounted in a volume.

#### Environment variable

- `CELLS_NO_SSL`: uses ssl or not (default to 0 => uses ssl)
- `CELLS_BIND` : internal url - used by services to talk between each others
- `CELLS_EXTERNAL` : external url - used in links, etc..

Note : CELLS_BIND and CELLS_EXTERNAL can only use different ports at the time of writing. They must share the same host.

### Example setup with docker compose

```yaml
version: '3'
services:

    # Cells image with two named volumes for the static and for the data
    cells:
        image: pydio/cells:latest
        restart: always
        volumes: ["static:/root/.config/pydio/cells/static/pydio", "data:/root/.config/pydio/cells/data"]
        ports: ["8080:8080"]
        environment:
            - CELLS_BIND=localhost:8080
            - CELLS_EXTERNAL=localhost:8080
            - CELLS_NO_SSL=1

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
         ports: ["3306:3306"]

volumes:
    static: {}
    data: {}
```

### Public access

#### HTTPS

We recommend you [run behind a proxy](https://pydio.com/fr/docs/cells/v1/run-behind-proxy) to encrypt the content you want to publish over the internet.

The [nginx-proxy](https://github.com/jwilder/nginx-proxy) and [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) containers can also be used to setup your proxy.
