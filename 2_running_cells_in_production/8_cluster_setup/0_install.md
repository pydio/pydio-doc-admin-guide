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
<li><p>Add the Pydio Cells Helm Chart repository<br> <code>$ helm repo add cells https://download.pydio.com/pub/charts/helm</code></p></li>
<li><p>Run the install command<br> <code>$ helm install cells cells/cells --namespace cells --create-namespace</code></li>
<li><p>The output will tell you how to access your app once the deployment is ready. It can take a few minutes.</p></li>
</ol>

## Go further

The default helm chart will deploy a Cells instance w/ dependencies but still being capable of running on the smallest of environment such as *minikube*.

The main parameters required to achieve a production-ready deployment are described below.

### values.yaml

<pre>
<code>

# NOTE : Cells Enterprise users can comment out the lines following the # [ED] comment

# Define what image version of Cells you want to use to have more control over your update
image:
  # [ED]
  # repository: pydio/cells-enterprise
  tag: latest

# Achieve high availability by starting a minimum number of replicas of the Cells stateless Pod
# NOTE: each dependency of Cells has their own high availability strategy
# Achieve horizontal scalability by setting up an autoscaling strategy 
autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 5
  targetCPUUtilizationPercentage: 80
  targetMemoryUtilizationPercentage: 80

resources:
  limits:
    cpu: "500m"
    memory: "2G"

# Achieve public-facing deployment by adding Ingress w/ Nginx as a load balancer
# Uses lets-encrypt as a certficate authority
ingress:
  enabled: true
  clusterissuer:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: myexampleaddress@gmail.com
  annotations: {
    "kubernetes.io/ingress.class": "nginx",
    "cert-manager.io/cluster-issuer":"letsencrypt"
  }
  pathType: Prefix
  hostname: cluster.pydiocells.com
  tls: true
  selfSigned: true

service:
  type: ClusterIP
  # [ED]
  # customconfigs: {
  #  "defaults/license/data": "MYLICENSE",
</code>
</pre>

Just run the following command in order to install and activate all the parameters defined above :

<ol class="install-steps">
<li><code>$ helm update cells cells/cells --namespace cells -f values.yaml</code></li>
</ol>

With sufficient resource, this will transform your basic non-secured system to a letencrypt-secured load balancer in front of a highly available and horizontally scalable Cells deployment. 

## Not sufficient yet ?

Read on the rest of the pages in this section to get a deeper understanding of how things work in a Pydio Cells cluster and find the configurations you need.