
At this step, you have a running instance of Pydio Cells running on your server. You can now connect as an administrator to fine tune the installation and start customising the tool to fit with your specific organisation's needs actual business data. 

In this section we will focus on two very important concepts of Pydio Cells **datasources** and **worspaces** that bring you power and flexibility. 

A user with admin rights can configure datasources and workspaces by going to `Top Menu >> Settings >> Data Management section`.

## Datasources 

_TODO enhance_

Datasources are the connectors to your physical data repositories. 
With Pydio Cells, we put a strong focus on conforming to standards and we use widely spread APIs: thus, datasources are a generic interface that conforms to the Amazon S3 protocol.  

By defining a datasource, you tell the system where to effectively find your data. 

For the time being, you can use either:
- Filesystem datasources: your files are stored in a file system, locally or on the network share
- Amazon S3 object storage

You will find more information on both storage in the dedicated sub-sections.

## Workspaces 

Another of the core concepts of Pydio Cells are the **"Workspaces"**. Basically, a workspace can be seen as a virtual drive mounted to access a set of data. 

In most case, a workspace will access a set of folders and files, locally or remotely via various types of protocol, but it can also be an access to other type of data, like a database content (MySQL repositories), or even the Pydio configurations themselves (Settings workspace). The way the files are accessed is defined when creating the workspace, by choosing a **"Datasource"** to map the data.

With this version of Pydio, we have brought the **Workspace** concept to the next level by introducing the concept of **Cells**: basically, you can now define virtual workspaces that provide direct access to any set of heterogeneous sources that can be:

- a set of workspace from various datasources 
- a single folder from within a given workspace
- a single file 

Cells are then easily sharable between users and groups and bring the ability to then define fine grained permissions, policies and tasks on a given set of data from these heterogeneous sources.

One of the cool features of cells is that it comes with no (or only little) additional costs in term of storage used: we do not copy the files but rather maintain sets of _shortcuts_ that are transparent to the end users but and point to the effective data.

The mapping is maintenained via the main internal index. See Overall architecture [documentation](/en/docs/cells/v1/pydio-cells-internals-0) to learn more about these concepts.

[:summary]