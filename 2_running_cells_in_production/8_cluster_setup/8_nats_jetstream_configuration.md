Cells microservices communicate using a pub/sub service. However, when a high volume of messages is produced, a queuing mechanism is required to manage resource consumption efficiently and ensure consistency.

In a standalone deployment, Cells uses a built-in queue, storing messages locally on disk.

In a clustered environment where Cells runs in stateless mode, it relies on NATS JetStream. However, by default, the NATS requires additional configuration to activate JetStream in cluster environment

# Configure CELLS using NATs JetStream
In a cluster environment where Nats is running, you can enable Nats jetstream queue by adding `CELLS_PERSISTQUEUE` env

For instance:

```
CELLS_PERSISTQUEUE=nats://nats.domain.com
```

# NATs Jetstream in Kubernetes

To enable NATS JetStream in Kubernetes, you need to modify two files:

* Cells deployment.yaml in cells helm chart version <= 0.1.2 
* Cells Helm chart values.yaml

## 1. Update deployment.yaml

Add an extra environment variable (CELLS_PERSISTQUEUE) to instruct Cells to use the NATS service as a queue.
Modify the containers section as follows:

```
containers:
  - name: {{ .Chart.Name }}
    args:
      ['-c', 'source /var/cells-install/source && cells start ']
    env:
      - name: POD_NAME
        valueFrom:
          fieldRef:
            fieldPath: metadata.name
      - name: CELLS_PERSISTQUEUE
        value: {{ include "cells.natsURL" . }}

```

## 2. Update values.yaml

When NATS starts with JetStream, it transitions from a Deployment to a StatefulSet. This change requires adding a PersistentVolume to the cluster.
An example configuration for NATS in values.yaml:

```
nats:
  enabled: true
  jetstream:
    enabled: true
    maxMemory: 1G
  auth:
    enabled: false
  volumePermissions:
    enabled: true

  # Allow pod to write to the mounted repository
  podSecurityContext: { enabled: true }

  persistence:
    enabled: true 
    storageClass: gp2
    annotations: {}
    accessModes:
      - ReadWriteOnce
    size: 8Gi
    selector: {}
  debug:
    enabled: true
  resourceType: statefulset

  ## Number of NATS nodes
  replicaCount: 3
  cluster:
    name: nats
    connectRetries: ""
    auth:
      enabled: false
      user: nats_cluster
      password: "tataP@ssdslfj12"

```      

> Note: The podSecurityContext: { enabled: true } setting is required for proper functionality.
