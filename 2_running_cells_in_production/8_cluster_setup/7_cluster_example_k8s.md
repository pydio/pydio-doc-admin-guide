This section shows how to run Cells in a multi-node setup using inside Kubernetes.

## What is Kubernetes ?

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

### Install a Kubernetes cluster

You can manually [create your own kubernetes cluster](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) on your own servers by using kubernetes tools.

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

The cells helm charts can be used to deploy a ReplicaSet of Cells stateless servers. Using helm3 tou can add the Cells Helm repo as follows :

```
helm repo add cells https://download.pydio.com/pub/charts/helm
helm install --namespace <namespace> --create-namespace my-cells cells/cells
```

------------------------

## Dependencies

Cells Chart declares the following dependencies :

Repo bitnami (https://charts.bitnami.com/bitnami)
- [mariadb](https://github.com/bitnami/charts/tree/master/bitnami/mariadb)
- [redis](https://github.com/bitnami/charts/tree/master/bitnami/mariadb)
- [nats](https://github.com/bitnami/charts/tree/master/bitnami/mariadb)
- [mongodb](https://github.com/bitnami/charts/tree/master/bitnami/mariadb)
- [minio](https://github.com/bitnami/charts/tree/master/bitnami/mariadb)

Repo Hashicorp (https://helm.releases.hashicorp.com)
- [vault](https://www.vaultproject.io/docs/platform/k8s/helm/configuration)

Each dependency parameter can be configured directly from the command line by adding the name of the dependency as prefix :

```
helm install my-cells cells/cells --set mariadb.image.tag=latest ...
```

Dependencies can also be ***disabled*** if you want to use your own deployment from another repo. You need to make sure that you set the corrected address of your external service in the cells configuration for cells to be able to reach it. 

```
helm install my-cells cells/cells --set mariadb.enabled=false
```

------------------------

## Configuration

### Number of replicas

| Parameter    | Description                                           | Default      |
| replicaCount | Number of replicas used be default by the application | 1            |

### Server image

| Parameter        | Description  | Default      |
| image.repository |              | pydio/cells  |
| image.pullPolicy |              | IfNotPresent |
| image.tag        |              | unstable     |

### Service

| Parameter        | Description  | Default      |
| service.type     |              | NodePort     |
| service.port     |              | 8080         |
| service.discoveryPort |         | 8002         |

## Resources

Resources are not set by default so that they can run on any environment (typically minikube). Feel free to set any limit you want 

```
resources: {}
  # We usually recommend not to specify default resources and to leave this as a conscious
  # choice for the user. This also increases chances charts run on environments with little
  # resources, such as Minikube. If you do want to specify resources, uncomment the following
  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  # limits:
  #   cpu: 100m
  #   memory: 128Mi
  # requests:
  #   cpu: 100m
  #   memory: 128Mi
```

## Autoscaling

Autoscaling is disabled by default. But you can enable it to have the replica set horizontally scaling to use the full (or as defined) capacity of your cluster.

```
autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80
  # targetMemoryUtilizationPercentage: 80
```

## Extra environment variables.

You can set extra environment variable in each cells pod :

```
extraEnvVars:
  - name: CELLS_WORKING_DIR
    value: /tmp/cells
```

## Ingress

In order to access your application remotely, you can set an ingress API object that will provide load balancing, SSL termination and name-based virtual hosting :

| Parameter           | Description                | Default      |
| ingress.enabled     |	Enables Ingress	           | false |
| ingress.labels	  | Ingress labels	           | {} |
| ingress.annotations | Ingress annotations	       | {} | 
| ingress.hosts	      | Ingress accepted hostnames | [] |
| ingress.tls	      | Ingress TLS configuration  | [] |