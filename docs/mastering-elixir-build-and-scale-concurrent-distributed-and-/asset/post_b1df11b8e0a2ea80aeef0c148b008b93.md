
Now, if you start the application as usual with `docker-compose -f docker-compose.dev.yml up`, you can go to http://localhost:9090/graph and look for some metrics:

![](img/c636f4cfb07f25f1db535995e50706ed_516_0.png)

With the local environment up and running, let's configure our cloud to also host a Prometheus server (and some other goodies).

## Deploying Prometheus in Kubernetes

As we did with our application in the previous chapter, we could configure a pod based on the Prometheus Docker image and call it a day. It would work, but whenever the Prometheus pod rotated, we would lose all the metrics because pods are ephemeral by nature. To have a proper monitoring solution in place, we should deploy Prometheus as the stateful application that it is, and utilize the abstractions that Kubernetes provides, such as StatefulSets and PersistentVolumes, among others.

However, doing this is not so trivial, so we will instead use Helm to install a chart that will take care of correctly deploying and configuring Prometheus and its companions in our Kubernetes cluster for us. You can think of Helm as a package manager (such as apt, chocolatey, or homebrew) for Kubernetes. A chart is nothing more than a bundle of Kubernetes templates (a sort of Kubernetes recipe) that Helm lets us install in our cluster. To use Helm, we also need its server-side counterpart: Tiller.

Tiller will run on the cluster and will install the charts according to the instructions provided by Helm running on your machine. Let's start by creating a tiller-sa ServiceAccount that will be used by Tiller:

```
$ kubectl -n kube-system create serviceaccount tiller-sa
serviceaccount "tiller-sa" created
```

Here's the role binding that will associate the previous service account to the cluster-admin role, so that Tiller has all the permissions it may need:

```
$ cat monitoring/tiller-sa-role-binding.yml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller-role-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller-sa
    namespace: kube-system
```

Now, we just need to apply the previous ClusterRoleBinding resource:

```
$ kubectl create -f monitoring/tiller-sa-role-binding.yml
clusterrolebinding.rbac.authorization.k8s.io "tiller-role-binding" created
```

With the tiller-sa service account created, we can move on and install Helm and its brother, Tiller. Follow the instructions at https://docs.helm.sh/using_helm/#installing-helm and you should end up with the helm binary somewhere in your path. Because we have the kubectl context configured to access our GKE cluster, helm will utilize this context to interact with the cluster and install its server-side component in one fell swoop:

```
$ helm init --service-account tiller-sa
Creating /home/andre/.helm
Creating /home/andre/.helm/repository
Creating /home/andre/.helm/repository/cache
Creating /home/andre/.helm/repository/local
Creating /home/andre/.helm/plugins
Creating /home/andre/.helm/starters
Creating /home/andre/.helm/cache/archive
Creating /home/andre/.helm/repository/repositories.yaml
Adding stable repo with URL:
https://kubernetes-charts.storage.googleapis.com
Adding local repo with URL: http://127.0.0.1:8879/charts
$HELM_HOME has been configured at /home/andre/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.
```

After this, we can see the tiller-deploy pod running in the kube-system namespace:

```
$ kubectl get pods --all-namespaces
NAMESPACE NAME READY STATUS RESTARTS AGE
# ...
kube-system tiller-deploy-75d774db4f-fbxch 1/1 Running 5 3d
```

We will install two Helm charts: prometheus-operator and kube-prometheus. These two charts are provided by CoreOS and are being actively developed. The kube-prometheus chart uses the Kubernetes resources introduced by the prometheus-operator one, and simplifies the management of Prometheus and Grafana (among other tools) in a Kubernetes cluster (for example, the configuration of persistent storage, safe coordination of application upgrades, disaster recovery, and so on).

Besides Prometheus, we will also use Grafana. Grafana is an open source data visualization tool that is able to consume Prometheus metrics (among other data sources) and use them to create graphs and dashboards.

We first need to install the CoreOS repository, so that helm can then find the charts we need:

```
$ helm repo add coreos
https://s3-eu-west-1.amazonaws.com/coreos-charts/stable/
"coreos" has been added to your repositories
```

Now, it is just a matter of installing both charts on a specific monitoring namespace. We're using a different namespace to keep everything tidy, but the Prometheus server will still be able to monitor our ElixirDrip pods running in their production namespace:

```
$ helm install coreos/prometheus-operator --name prometheus-operator --namespace monitoring
NAME: prometheus-operator
LAST DEPLOYED: Wed Jun 27 19:51:49 2018
NAMESPACE: monitoring
STATUS: DEPLOYED
  # ...
```

```
$ helm install coreos/kube-prometheus --name kube-prometheus --namespace monitoring
NAME: kube-prometheus
LAST DEPLOYED: Wed Jun 27 20:04:00 2018
NAMESPACE: monitoring
STATUS: DEPLOYED
  # ...
```

```
$ kubectl -n monitoring get pods
NAME READY STATUS RESTARTS AGE
alertmanager-kube-prometheus-0 2/2 Running 4 2h
kube-prometheus-exporter-kube-state-5ffd856844-m6wbj 2/2 Running 0 1h
kube-prometheus-exporter-node-4ddt6 1/1 Running 2 2h
kube-prometheus-exporter-node-ppj2g 1/1 Running 0 1h
kube-prometheus-grafana-6c9496d766-q7278 2/2 Running 4 2h
prometheus-kube-prometheus-0 3/3 Running 1 2h
prometheus-operator-f454cb555-htqxx 1/1 Running 2 2h
```

After a while, you will be able to see all pods created by both charts running in the monitoring namespace.

![](img/c636f4cfb07f25f1db535995e50706ed_519_0.png)

When I tried to deploy the two Helm charts on a GKE cluster with a single n1-standard-1 node (1 vCPU and 3.75 GB of RAM), some of the pods weren't able to start, and an unschedulable pods notification appeared in the Google Cloud console. After activating the cluster autoscaler and setting its maximum size to 2, Kubernetes automatically scaled the cluster up to make room for the new pods.

If we forward port 9090 of the `prometheus-kube-prometheus-0` pod, we will be able to access the Prometheus server we've just deployed:

```
$ kubectl -n monitoring port-forward prometheus-kube-prometheus-0 49090:9090
Forwarding from 127.0.0.1:49090 -> 9090
Forwarding from [::1]:49090 -> 9090
# ...
```

If you now go to http://localhost:9090/graph, you will see the same Prometheus page that we accessed locally before. However, we still don't have any ElixirDrip metrics, because we haven't yet configured Prometheus to scrape the metrics exposed by the application's `/metrics` endpoint.

To scrape metrics from our application, we don't need to directly update the Prometheus configuration file, because `prometheus-operator` introduces a custom Kubernetes resource called `ServiceMonitor`, whose sole purpose is to configure how Prometheus should find available targets.

Before configuring our own `ServiceMonitor`, let's see the `ServiceMonitors` created by the operator for us:

```
$ kubectl -n monitoring get servicemonitors
NAME AGE
kube-prometheus 3h
kube-prometheus-alertmanager 3h
kube-prometheus-exporter-kube-controller-manager 3h
kube-prometheus-exporter-kube-dns 3h
kube-prometheus-exporter-kube-etcd 3h
kube-prometheus-exporter-kube-scheduler 3h
kube-prometheus-exporter-kube-state 3h
kube-prometheus-exporter-kubelets 3h
kube-prometheus-exporter-kubernetes 3h
kube-prometheus-exporter-node 3h
kube-prometheus-grafana 3h
prometheus-operator 3h
```

It's the `kube-prometheus-exporter-kubelets` service monitor that tells Prometheus how to scrape metrics from Kubernetes. By default, Prometheus fetches Kubernetes metrics in a secure way. However, due to how Google Kubernetes Engine works, we need to change the service monitor for this target:

```
$ kubectl -n monitoring get servicemonitor kube-prometheus-exporter-kubelets -o yaml | sed 's/https/http/' | kubectl replace -f -
servicemonitor.monitoring.coreos.com "kube-prometheus-exporter-kubelets" replaced
```

In the previous snippet, we get the YAML definition of `kube-prometheus-exporter-kubelets` ServiceMonitor, replace its `https` occurrences with `http`, and then replace its resource with the new HTTP-only version. With this change, Prometheus is able to scrape all the targets configured by the operator that came out of the box.

The `ServiceMonitor` resource we've just updated is a custom resource introduced by `prometheus-operator`. It enables us to configure Prometheus targets, without actually having to update any Prometheus configuration, by relying on similar Kubernetes concepts, such as label selectors. Here's the `ServiceMonitor` definition that we'll create to set ElixirDrip as a Prometheus target:

```
$ cat monitoring/elixir-drip-service-monitor-prod.yml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  labels:
    app: prometheus
  name: elixir-drip-monitor
  namespace: monitoring
spec:
  jobLabel: elixir-drip
  endpoints:
  - interval: 30s
    port: web
  namespaceSelector:
    matchNames:
    - production
  selector:
    matchLabels:
      app: elixir-drip
```
