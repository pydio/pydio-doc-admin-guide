_This section covers the basics of the Pydio Cells architecture. Although not necessary, we believe it's a good read to understand what is going on under the hood and thus, among others, have a better understanding of the log messages_

## Introduction

Pydio Cells is composed of a set of **micro-services**, which are small, mono-oriented, independant programs communicating together only via defined API's. This does improve flexibility and overall scalability of the system.

This is transparent for standard, one-node deployments: when you run Pydio Cells with the standard `./cells start` command, all services are started altogether, and a dedicated "proxy" service provides the glue to expose only one HTTP address to the users. You certainly do not "have to" run each service independantly on different machines. 

### Services are stateless and fully independants

As exposed, each micro service exposes its own API, using either a gRPC Protocol or a REST HTTP protocol (more on that later). Also, each micro service comes with its own storage. Some services rely on a MySQL storage, whereas others currently embed their own on-file database (based on BoltDB). This is important as it allows to scale each service independantly depending on the load applied on this service. 

Again, the complexity is hidden from you for standard deployments: the central configuration shares one database for all services. Inside this database, each service creates its own tables and relies only on these tables. There are *no relational links* betweens the tables of different services. By correctly tweaking the configuration file, you can assign a different database server for each server without issue.

### Communication and main service types

Services communicate one with another via a fixed API. There are two types of services, following the "gateway" pattern of micro-services best practices.

- __pydio.grpc.*__ services: low-level layers, interacting with the persistence layer and implementing very basic CRUD operations for objects, are exposing a **GRPC** API. Using **Protocol Buffers**, this remote procedure call system based on HTTP2 provides high efficiency, streaming and many other advantages. It is promoted by Google and massively adopted in any language. Protobuf is a standard syntax for describing messages and services, and a specific tool allows the translation of these protobufs into concrete implementation in the language of your choice, for both servers and clients.  
GRPC services are never connected directly from the outside world (like http browsers).

- __pydio.rest.*__ All interactions with the outside world are performed using **HTTP/1 Rest APIs**. These services receive HTTP calls forwarded by the gateway, and forward them to the underlying services. The idea of micro-service is to deport the logic as high as possible in the client. Thus where the GRPC services are generally very basic CRUD providers, the REST services may provide more business logic and create glue between many GRPC services.  

These are the two main service types that Pydio Cells is running. They are often running as pairs: exposing some objects to the outside world requires a dedicated "rest" gateway even when no intelligence is required. 

You can for instance find following services in Pydio Cells:  
`pydio.grpc.policies` [storing policies in DB]  => `pydio.rest.policies` [providing rest apis for listing/creating policies].

## Global Architecture

Below is the global architecture schema (not all services are represented here):

[:image-popup:5_advanced/cells_architecture.png]

## Services Tags

As you can see on the schema above, services are also grouped by their logical category. This is just a naming convention and has no technical impact. These groups are tags attached to services, and can be used to organize information when listing services. 

This is the case when you use `./cells list` command :

```sh
 GENERIC SERVICES                                      
 # discovery                                           
 nats                                       [X]        
 # frontend                                            
 pydio.api.front-plugins                    [X]        
 # gateway                                             
 micro.api                                  [X]        
 pydio.api.websocket                        [X]        
 pydio.rest.gateway.dav                     [X]        
 pydio.rest.gateway.wopi                    [X]        
                                                       
 GRPC SERVICES                                         
 # broker                                              
 pydio.grpc.activity                        [X]        
 pydio.grpc.chat                            [X]        
 pydio.grpc.log                             [X]        
 pydio.grpc.mailer                          [X]        
 # data                                                
 pydio.grpc.data-key                        [X]        
 pydio.grpc.docstore                        [X]        
 pydio.grpc.meta                            [X]        
 pydio.grpc.search                          [X]        
 pydio.grpc.tree                            [X]        
 pydio.grpc.versions                        [X]        
 # datasource                                          
 pydio.grpc.data.index                      [X]        
 pydio.grpc.data.index.cells                [X]        
 pydio.grpc.data.index.newdatasource        [X]        
 pydio.grpc.data.index.personal             [X]        
 pydio.grpc.data.index.pydiods1             [X]        
 pydio.grpc.data.index.s3                   [X]        
 pydio.grpc.data.objects                    [X]        
 pydio.grpc.data.objects.gateway1           [X]        
 pydio.grpc.data.objects.local1             [X]        
 pydio.grpc.data.objects.local2             [X]        
 pydio.grpc.data.sync                       [X]        
 pydio.grpc.data.sync.cells                 [X]        
 pydio.grpc.data.sync.newdatasource         [X]        
 pydio.grpc.data.sync.personal              [X]        
 pydio.grpc.data.sync.pydiods1              [X]        
 pydio.grpc.data.sync.s3                    [X]        
 # discovery                                           
 pydio.grpc.config                          [X]        
 pydio.grpc.update                          [X]        
 # gateway                                             
 pydio.grpc.gateway.data                    [X]        
 pydio.grpc.gateway.proxy                   [X]        
 # idm                                                 
 pydio.grpc.acl                             [X]        
 pydio.grpc.auth                            [X]        
 pydio.grpc.policy                          [X]        
 pydio.grpc.role                            [X]        
 pydio.grpc.share                           [X]        
 pydio.grpc.user                            [X]        
 pydio.grpc.user-key                        [X]        
 pydio.grpc.user-meta                       [X]        
 pydio.grpc.workspace                       [X]        
 # scheduler                                           
 pydio.grpc.jobs                            [X]        
 pydio.grpc.tasks                           [X]        
 pydio.grpc.timer                           [X]        
                                                       
 REST SERVICES                                         
 # broker                                              
 pydio.rest.activity                        [X]        
 pydio.rest.log                             [X]        
 pydio.rest.mailer                          [X]        
 # data                                                
 pydio.rest.docstore                        [X]        
 pydio.rest.meta                            [X]        
 pydio.rest.search                          [X]        
 pydio.rest.tree                            [X]        
 # discovery                                           
 pydio.rest.config                          [X]        
 pydio.rest.update                          [X]        
 # frontend                                            
 pydio.rest.frontend                        [X]        
 # idm                                                 
 pydio.rest.acl                             [X]        
 pydio.rest.auth                            [X]        
 pydio.rest.graph                           [X]        
 pydio.rest.policy                          [X]        
 pydio.rest.role                            [X]        
 pydio.rest.share                           [X]        
 pydio.rest.user                            [X]        
 pydio.rest.user-meta                       [X]        
 pydio.rest.workspace                       [X]        
 # scheduler                                           
 pydio.rest.jobs                            [X]        
```

Let's explain briefly the roles of each groups of services:

- **Discovery**:  These are the core services necessary to run Pydio Cells. It includes among others the `nats` service, which allows all micro-services to auto-discover the others magically.
- **Gateway**: "Front" services facing the world
- **IDM**: as "Identity Management", all the authentication/authorizations related services
- **Scheduler**: Background jobs manager
- **Broker**: Loosely-coupled services mostly listening on events to trigger some actions, like storing user's feeds of activities, sending emails, etc..
- **DataSource**: DataSource services (objects / index / sync) as explained in the DataSource [chapter](/en/docs/cells/v1/understanding-datasources).
