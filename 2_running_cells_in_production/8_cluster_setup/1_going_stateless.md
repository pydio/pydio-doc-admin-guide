This section is a must-read to understand what you are doing when deploying Cells in cluster mode.

## Deploying software in a distributed environment

A key to deploying software in a distributed environment is the ability to easily replicate a running instance of the software while being sure that any new clone will share data and communicate with existing ones: 

- Providing **high-availability** is about ensuring that if a running node is broken, another one can easily take over. 
- Providing **horizontal scalability** is about ensuring that if a running node is overloaded, one can easily add a new one and balance the load on it. 

To achieve such flexibility, DevOps generally use pre-packaged software "images" (generally a _virtual machine_, or a _container_) that are isolated one from another in terms of CPU and memory, but working inside a private network to communicate with each other. It is crucial that these running images do not self-contain any business data that would be inacessible to any other instances.


## Cells default setup limitation 

For example, a standard Cells setup inside a VM generates logs, search indexes, audits, etc. directly on-file (BoltDB). An approach can be to share a mounted filessystem accross various VMs, but that quickly leads to locking issues and is poorly reliable. Porting these storages instead to external, network-accessed dependencies, makes the data accessible by all images and tackle this issue: that specific BoltDB can be replaced with a MongoDB connection.

If we go deep into the structure of Cells-generated data, we can find many types of them, associated to many storages: 

- Configuration (pydio.json)
- Logs, search indexes, etc.. (BoltDB/BleveSearch indexes)
- Services Registry (in-memory process and grpc communication with forks)
- Pub/sub events (in-memory process and grpc communication with forks), 
- Etc... 

[:image-popup:2_running_cells_in_production/cluster/cells-statefull.png]

## A Cells stateless deployment

As you can see, going stateless here will consist of deploying the necessary third-party tools to **unload this data from Cells internal to shared stores** that provide their own clustering mechanism for redundancy and failover.  The image below shows the technologies involved to tackle the issue.

[:image-popup:2_running_cells_in_production/cluster/cells-stateless-1.png]

In a more concise view, we now have : 

[:image-popup:2_running_cells_in_production/cluster/cells-stateless-2.png]