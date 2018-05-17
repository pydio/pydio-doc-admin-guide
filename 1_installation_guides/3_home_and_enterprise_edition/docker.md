_This guide describes the steps required to have the Pydio Cells Docker container running._

[:image-popup:1_installation_guides/logos-os/logo-docker.png]

### Pydio's Cell docker image

You can get our image [from our docker hub](https://hub.docker.com/r/pydio/cells/)
or use the following command `docker pull pydio/cells`.

Now for how to run and use this image we will provide you with examples, and situations that will facilitate this process.

This is the base docker command:

```sh 
docker run -it -d --name cells -t -v /<localstorage>/:/root/.config/pydio/cells/ -e "CELLS_BIND=<address>:<port>" -e "CELLS_EXTERNAL=<address>:<port>" -p 8080:8080 pydio/cells
```

For the details:

* -it : interactive
* -d : detached
* -t : allocate a pseudo TTY
* -v : volume bind in this order `<local>:<inside container>`
* -e : are ENV variable in this order `<which ENV variable will have that value inside the container>:<value>`
* -p : port bind in this order` <outside>:<inside container>`

So, for instance: 

```sh 
docker run -it -d --name cells -t -v /home/my_pc/documents:/root/.config/pydio/cells/ -e "CELLS_BIND=192.168.0.198:8080" -e "CELLS_EXTERNAL=192.168.0.198:8080" -p 8080:8080 pydio/cells
```

Make always sure that the -p `<outside>:` is the same as the `CELLS_EXTERNAL` and the -p `:<inside>` as `CELLS_BINDS`.

For the volume part -v `<localstorage>` use a writable and readable volume.

To access your cell after the deployement use this type of url :
`https://<address>:<port>`, beware it's `https` and not `http`.
