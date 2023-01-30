<style type="text/css">
ol.numbering {
  counter-reset: my-awesome-counter;
}

ol.numbering li {
  counter-increment: my-awesome-counter;
}

ol.numbering li:before {
  content: counter(my-awesome-counter) ". ";
  color: #44d2ab;
  font-weight: bold;
  margin-right: 10px;
  font-size: 22px;
}

ol.install-steps {
  padding-left: 0 !important;
  list-style: none;
  padding: 0;
  margin:0;
}
ol.install-steps li {
  border-left: 2px solid #08cc99;
  display:flex;
  align-items: baseline;
  background-color: #ecf8f6;
  padding: 16px 20px;
  margin: 20px 0 !important;
}

ol.install-steps li p {
  display: inline;
  width: 100%;
  margin: 0 !important;
  font-size: 18px !important;
}

ol.install-steps li code {
    font-size: 16px !important;
    display: block;
    margin: 0 !important;
    padding: 15px 30px 15px 15px !important;
    background-color: rgb(42 42 53 / 95%) !important;
    color: white !important;
    margin-top: 6px !important;
}

ol span.geshifilter {
    display: inherit;
}

</style>

_These commands deploy a fully runnable instance of Cells on Kubernetes. Use it to quickly spin off a production-ready environment having high-availability and horizontal scalability at the ready from the get-go_

<ol class="install-steps numbering">
<li><p>Add the Pydio Cells Helm Chart repository<br> <code>$ <span>helm repo add cells https://download.pydio.com/pub/charts/helm</span></code></p></li>
<li><p>Run the install command<br> <code>$ <span>helm install cells cells/cells --namespace cells --create-namespace</span></code></li>
</ol>

## Go further

The default helm chart will deploy a Cells instance w/ dependencies but still being capable of running on the smallest of environment such as *minikube*. All the parameters needed to achieve a production-ready deployment are described below. You can go even further than that in your understanding of how things are working under the bonnet.

### values.yaml

<pre>
<code>
# Define what image version of Cells you want to use to have more control over your updates
image:
  pullPolicy: Always
  tag: latest

# Achieve high availability by starting multiple replicas of the Cells stateless Pod
# NOTE: each dependency of Cells has their own high availabilty strategy 
replicaCount: 3

# Achieve public-facing deployment by adding Ingress w/ Nginx as a load balancer
# Uses lets-encrypt as a certficate authority
ingress:
  enabled: true
  annotations: {
    "kubernetes.io/ingress.class": "nginx",
    "cert-manager.io/cluster-issuer":"letsencrypt"
  }
  pathType: Prefix
  hostname: cluster.pydiocells.com
  tls: true

service:
  type: ClusterIP

# Achieve horizontal scalability by setting up an autoscaling strategy 
autoscaling:
  enabled: true
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80
  targetMemoryUtilizationPercentage: 80
</code>
</pre>

Just run the following command in order to install and activate all the parameters defined above :

<ol class="install-steps">
<li><code>$ helm update cells cells/cells --namespace cells -f values.yaml</code></li>
</ol>
### Ingress

### High availability

### Horizontal scalability

### TLS transport

### 