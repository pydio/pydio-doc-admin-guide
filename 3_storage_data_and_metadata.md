# Storage, data and metadata

At this step, you have a running instance of Pydio Cells running on your server. You can now connect as an administrator to fine tune the installation and start customising the tool to fit with your specific organisation's needs actual business data. 

In this section we will focus on two very important concepts of Pydio Cells **datasources** and **worspaces** that bring you power and flexibility. 

A user with admin rights can configure datasources and workspace by going to `Top Menu >> Settings >> Data Management section`.

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

Another of the core concept of Pydio Cells is the **"Workspace"**. Basically, a workspace can be seen as a virtual drive mounted to access a set of data. 

In most case, a workspace will access a set of folders and files, locally or remotely via various types of protocol, but it can also be an access to other type of data, like a database content (MySQL repositories), or even the Pydio configurations themselves (Settings workspace). The way the files are accessed is defined when creating the workspace, by choosing a **"datasource"** to map the data.

With this version of Pydio, we have brought the **workspace** concept to the next level by introducing the concept of **Cell**: basically, now you can define virtual workspaces that provide direct access to any set of heterogeneous sources that can be:

- a set of workspace
- a single folder from within a given workspace
- a single file 

Cells are then easily sharable between users and groups and bring the ability to then define fine grained permissions, policies and tasks on a given set of data from heterogeneous sources.

One of the cool feature of the cells is that it comes almost without any additional costs in term of storage used: we do not copy the files but rather maintain sets of _shortcuts_ that are transparent to the end users but point to the effective data.


[:summary]