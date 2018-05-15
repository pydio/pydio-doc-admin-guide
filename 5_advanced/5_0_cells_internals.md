_This section covers the basics of the Pydio Cells architecture. Although not necessary, we believe it's a good read to understand what's going on under the hood, and particularly have a better understanding of the log messages_

## Introduction

Pydio Cells is composed under the hood of a set of **micro-services**, which are small, mono-oriented, independant programs communicating together only via defined API's. This does improve flexibility and overall scalability of the system.

This is transparent for standard, one-node deployments: when you run Pydio Cells with the standard `./cells start` command, all services are started altogether, and a dedicated "proxy" service provides the glue to expose only one HTTP address to the users. You certainly do not "have to" run each service independantly on different machines. 

### Services are stateless and fully independants

As exposed, each micro-services exposes its own API, using either a GRPC Protocol or a REST Http protocol (more on that later). Also, each micro-services comes with its own storage. Some services will rely on a MySQL storage, whereas some other currently embed their own on-file database (based on BoltDB). This part is very important as it allows to scale each service independantly depending on the load applied on this service. 

Again, the complexity is hidden to you for standard deployments as the central configuration will share one database for all services. But inside this database, each service will create its own tables and rely only on these tables. There are *no relational links* betweens the tables of different services. By correctly tweaking the configuration file, you can assign a different database server for each server without problems.

### Communication and main service types

Services communicate one with another via a fixed API. There are two types of services, following the "gateway" pattern of micro-services best practice.

- **pydio.grpc.*** services: low-level layers, interacting with the persistence layer and implementing very basic CRUD operations for objects, are exposing a **GRPC** API. Using **Protocol Buffers**, this remote procedure call system based on HTTP2 provides high efficiency, streaming and many other advantages. It is promoted by Google and massively adopted in any language. Protobuf is a standard syntax for describing messages and services, and a specific tool allows the translation of these protobufs into concrete implementation in the language of your choice, for both servers and clients.  
GRPC services are never connected directly from the outside world (like http browsers).

- **pydio.rest.*** All interactions with the outside world are performed using **HTTP/1 Rest APIs**. These are implemented by other services receiving http calls from outside, and forwarding them to the underlying services. The idea of micro-service is to deport the logic as high as possible in the client. Thus where the GRPC services are generally very basic CRUD provides, the REST services may provide more business logic and create glue between many GRPC services.  

These are the two main services types that Pydio Cells is running. They are often running as pairs: even when no intelligence is required, exposing some objects to the outside world requires a dedicated "rest" gateway. So you would find in Pydio the following services (for example): 

pydio.grpc.policies [storing policies in DB]  => pydio.rest.policies [providing rest apis for listing/creating policies].

## Global Architecture

