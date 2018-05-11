
As you might have already noticed, Pydio Cells web interface provides you with a rich user experience.
Yet at Pydio we love `sysadmins` (some of us even _are_ sysadmins). So we are doing our best to make their day-to-day job as smooth as possible.

Thus Pydio Cells comes out-of-the-box with a powerfull CLI (Command Line Interface) to control the services, integrate with the host system and perform admin tasks with no need for a Graphical Interface.

For instance, you can simply launch the server from a terminal by typing:

```sh
cd <path to your Pydio Cells binary file> 
# For Home Edition
./cells start  
# For Enterprise Edition
./cells-enterprise start  
```
_Note: it is **NOT** the correct way of launching the application when running in production mode._

In the rest of this section, when showing terminal commands, we will assume that:

- we are in the folder as the binary file _or_ that the application has been installed in the `PATH` of your operating system.
- we are using Pydio Cells Home Edition. For this matters, both versions are pretty the same. 

A last important remark before we get our hands dirty: **Pydio Cells** comes also with an extended command line tools, `cells-ctl` that is a simple but full fledge client that offers most of the important end-user features that can be found in the web interface. You will find more information about this tools in the corresponding section of the _Advanced_ chapter.      

### The Basics

Like the rest of the Pydio Cells User Interface, either graphical or not, this tool has a precise and very helpful documentation.
In case of doubt, you can always type:

`./cells --help`

or, for a specific subcommand:

`./cells start --help`

With the CLI, you can among other:

**Launch the installation process** (and optionnaly completely go through it, with thus no need for a graphical interface to install Pydio) 

`./cells install`

**Start and stop the server** or a given service:

```
# For instance, to enable debug mode
./cells start --log debug
# Start all services except one, here the dav server
./cells start -x pydio.rest.gateway.dav
```

**Display Configuration** of the running instance. 

`./cells config list`

That is among others handy to retrieve the default credential used by the front end to communicate with the backend. 

**List Services**: Pydio Cells is built following principles of the Micro Service architecture, having each part of the application communicating with the rest via messages and auto discovery using a registry. More on this in the corresponding section of this guide.

Simply typing:

`./cells list`

You can access a real time list of the services known by the application and there status.

For additional information, we strongly encourage you to refer to the manual page that can be found in-line simply typing: `./cells --help`
