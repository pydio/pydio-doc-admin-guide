Instead of managing manually your own cluster as in the precedent page, you might want to choose a container orchestration tool such as Kubernetes to have a maximum of flexibility on your deployment.
This page gives detailed information on how to run Cells in a multi-node setup using inside Kubernetes.

## What is Kubernetes ?

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

### Install a Kubernetes cluster

You can manually [create your own kubernetes cluster](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) on your servers by using kubernetes tools.

Alternatively, there are many operators that offer the possibility of installing a Kubernetes cluster easily. You can find a few below :

- [Amazon Elastic Kubernets Service (EKS)](https://aws.amazon.com/fr/eks/)
- [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine)
- [Microsoft Azure Kubernetes Service (AKS)](https://azure.microsoft.com/fr-fr/free/kubernetes-service/)
- [Scaleway Kubernetes Kapsule](https://www.scaleway.com/fr/kubernetes-kapsule/)

For testing, you can use [minikube](https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/) to easily deploy your applications in a local cluster

## What is HELM ?

Helm is essentially a package manager for kubernetes applications. Helm Charts can be defined to easily preconfigure the deployment of an application with its dependencies.

Parameters can be changed by setting them during install or upgrade. (e.g. ```helm install my-cells cells/cells --set image.tag=latest```)

## Kubectl

You can use kubectl locally to easily access your remote cluster. Change your [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) to manage and monitor your deployment directly from your computer.
Helm commands will automatically use the kubeconfig configuration.

## Install using Helm

Refer to the [quick install page](./kubernetes-quick-install) for more information.

The cells helm charts can be used to deploy a ReplicaSet of Cells stateless servers. Using helm3 you can add the Cells Helm repo as follows :

```
helm repo add cells https://download.pydio.com/pub/charts/helm
helm install --namespace <namespace> --create-namespace my-cells cells/cells
```

------------------------

## Dependencies

Each dependency parameter can be configured directly from the command line by adding the name of the dependency as prefix :

```
helm install my-cells cells/cells --set mariadb.image.tag=latest ...
```

Dependencies can also be ***disabled*** if you want to use your own deployment from another repo. You need to make sure that you set the corrected address of your external service in the cells configuration for cells to be able to reach it. 

```
helm install my-cells cells/cells --set mariadb.enabled=false
```

Cells Chart declares the following **mandatory** dependencies below. They are **all** necessary for a fully functional Cells cluster. You can install equivalent versions if you require by disabling the initial dependency

| Name                                                                     | Repo                                          | Enable            | Parameters list                                                 |
|--------------------------------------------------------------------------|-----------------------------------------------|-------------------|-----------------------------------------------------------------|
| [mariadb](https://github.com/bitnami/charts/tree/master/bitnami/mariadb) | [bitnami](https://charts.bitnami.com/bitnami) | `mariadb.enabled` | https://artifacthub.io/packages/helm/bitnami/mariadb#parameters |
| [redis](https://github.com/bitnami/charts/tree/master/bitnami/redis)     | [bitnami](https://charts.bitnami.com/bitnami) | `redis.enabled`   | https://artifacthub.io/packages/helm/bitnami/redis#parameters   | 
| [nats](https://github.com/bitnami/charts/tree/master/bitnami/nats)       | [bitnami](https://charts.bitnami.com/bitnami) | `nats.enabled`    | https://artifacthub.io/packages/helm/bitnami/nats#parameters    |
| [mongodb](https://github.com/bitnami/charts/tree/master/bitnami/mongodb) | [bitnami](https://charts.bitnami.com/bitnami) | `mongodb.enabled` | https://artifacthub.io/packages/helm/bitnami/mongodb#parameters | 
| [minio](https://github.com/bitnami/charts/tree/master/bitnami/minio)     | [bitnami](https://charts.bitnami.com/bitnami)   | `minio.enabled`   | https://artifacthub.io/packages/helm/bitnami/minio#parameters   |
| [vault](https://www.vaultproject.io/docs/platform/k8s/helm/configuration) | [Hashicorp](https://helm.releases.hashicorp.com) | `vault.enabled`    | https://developer.hashicorp.com/vault/docs/platform/k8s/helm/configuration |

Cells Chart declares the following **optional** dependencies below

| Name              | Repo         | Enable           | Parameters list |
|-------------------|--------------|------------------|-----------------|
| [ingress-nginx](https://github.com/kubernetes/ingress-nginx/tree/main/charts/ingress-nginx) | [kubernetes] | `ingress.enabled` | https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx#values |
------------------------

## Configuration

### Number of replicas

| Parameter    | Description                                           | Default      |
| --- | --- | --- |
| `replicaCount` | Number of replicas used be default by the application | 1            |

### Server image

| Parameter        | Description  | Default      |
| --- | --- | --- |
| `image.repository` |              | pydio/cells  |
| `image.pullPolicy` |              | IfNotPresent |
| `image.tag`        |              | unstable     |

### Service

| Parameter                | Description                                           | Default                                                                                                                                                                                                                                                                                                |
|--------------------------|-------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `service.type`           |                                                       | NodePort                                                                                                                                                                                                                                                                                               |
| `service.port`           |                                                       | 8080                                                                                                                                                                                                                                                                                                   |
| `service.discoveryPort`  |                                                       | 8002                                                                                                                                                                                                                                                                                                   |
| `service.binds`          | Configures new bind addresses for the pod             | *not set*                                                                                                                                                                                                                                                                                              |
| `service.reverseproxyurl` | Configure the reverse proxy url for the pod           | *not set*                                                                                                                                                                                                                                                                                              |
| `service.tlsconfig` | Configure the tlsconfig of the pod load balancer      | *not set*                                                                                                                                                                                                                                                                                              |
| `service.customconfigs` | Configure custom configuration for the Cells instance | {<br>&nbsp;# Initial license<br>&nbsp;"defaults/license/data": "FAKE",<br><br>&nbsp;# sticky session for grpc<br>&nbsp;"cluster/clients/grpc/loadBalancingStrategies[0]/name": "priority-local",<br><br>&nbsp;#<br>&nbsp;"frontend/plugin/core.pydio/APPLICATION_TITLE": "My Pydio Cells Cluster"<br>} |

## Resources

Resources are not set by default in order to run everywhere.

But it becomes mandatory if you want to set up an autoscaling strategy (below)

| Parameter                    | Description  | Default   |
|------------------------------| --- |-----------|
 | `resources.limits.cpu`       | | *not set* |
 | `resources.limits.memory`    | | *not set* |
 | `resources.requests.cpu`     | | *not set* |
| `resources.requests.memory`  | | *not set* |

## Autoscaling

Autoscaling is disabled by default. But you can enable it to have the replica set horizontally scaling to use the full (or as defined) capacity of your cluster.

| Parameter           | Description                                                                                               | Default |
|---------------------|-----------------------------------------------------------------------------------------------------------|---------|
| `autoscaling.enabled` | Enables autoscaling                                                                                       | false   |
| `autoscaling.minReplicas` | Minimum number of replicas started for a Cells deployment                                                 | 3       |
| `autoscaling.maxReplicas` | Maximum number of replicas started for a Cells deployment                                                 | 5       |
| `autoscaling.targetCPUUtilizationPercentage` | Target cpu percentage usage of the maximum resource allocated to reach to trigger a new pod deployment    | 80      |
| `autoscaling.targetMemoryUtilizationPercentage` | Target memory percentage usage of the maximum resource allocated to reach to trigger a new pod deployment | 80      |

## Ingress

In order to access your application remotely, you can set an ingress API object that will provide load balancing, SSL termination and name-based virtual hosting :

| Parameter                      | Description                              | Default     |
|--------------------------------|------------------------------------------|-------------|
| `ingress.enabled`              | 	Enables Ingress	                        | false       |
| `ingress.annotations`          | Ingress annotations	                     | {<br>"kubernetes.io/ingress.class": "nginx",<br>&nbsp;"cert-manager.io/cluster-issuer": "letsencrypt",<br>&nbsp;"nginx.ingress.kubernetes.io/proxy-body-size": "0"<br>}         | 
| `ingress.hostname`	            | Ingress main hostname                    | cells.local |
| `ingress.tls`	                 | Ingress TLS enabled                      | false       |
| `ingress.clusterissuer.server` | URL to the LetsEncrypt certification API | https://acme-v02.api.letsencrypt.org/directory |
| `ingress.clusterissuer.email` | Email used for verification during the certification | *not set* |
| `ingress.extraHosts` | Potential extra hostnames allowed | [] |
