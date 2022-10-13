As explained in the previous sections, Cells needs to externalize all storage and services layers to more easily provide a stateless, replicable image. Each of these external services are open source and can themselves be deployed in a highly-available setup. 

This page gathers information about each service and links to the documentation for their installation (that will depend on your cloud). Being a core requirement of Cells, MySQL/MariaDB is not handled here.

## ETCD : distributed configurations and services registry 

**etcd** is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or cluster of machines. It gracefully handles leader elections during network partitions and can tolerate machine failure, even in the leader node.

Cells uses Etcd for both sharing configuration acroos cluster nodes, and dynamic "registry" of all Cells services currently running in the cluster.

Learn more at [https://etcd.io/](https://etcd.io/)

## Hashicorp VAULT : secure storage for master keys and certificates

Manage Secrets & Protect Sensitive Data : secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API.

Cells uses Vault to replace the file-base keyring by a distributed and secure storage.

Learn more at [https://www.vaultproject.io/docs](https://www.vaultproject.io/docs)

## Nats.io : message broker

NATS is a connective technology built for the ever increasingly hyper-connected world. It is a single technology that enables applications to securely communicate across any combination of cloud vendors, on-premise, edge, web and mobile, and devices. NATS consists of a family of open source products that are tightly integrated but can be deployed easily and independently. NATS is being used globally by thousands of companies, spanning use-cases including microservices, edge computing, mobile, IoT and can be used to augment or replace traditional messaging.

Cells can connect to a Nats server for messages broadcasting inside the application, replacing the in-house gRPC broker used in simpler deployments.

Learn more at [https://docs.nats.io/](https://docs.nats.io/)

## Redis : shared cache

Redis is the open source, in-memory data store used by millions of developers as a database, cache, streaming engine, and message broker.

It is used by Cells for ephemeral and shared caching 

Learn more at [https://redis.io/docs/](https://redis.io/docs/)

## Mongo DB : NoSQL database
 
Mongo DB offers a flexible document data model along with support for ad-hoc queries, secondary indexing, and real-time aggregations to provide powerful ways to access and analyze your data.

It is used by many Cells services that are best-fitted for storing data in a NoSQL format.

Learn more at [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)
