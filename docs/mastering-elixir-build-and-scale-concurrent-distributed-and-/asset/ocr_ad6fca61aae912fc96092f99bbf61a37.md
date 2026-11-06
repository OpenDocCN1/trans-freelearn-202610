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