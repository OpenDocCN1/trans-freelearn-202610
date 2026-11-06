
We are setting the `app=prometheus` label in our custom ServiceMonitor because the `kube-prometheus` resource (of the `Prometheus` kind, also a custom resource introduced by the Prometheus operator) by default searches for ServiceMonitors with specific `app` label values (`alertmanager`, `grafana`, `prometheus`, and so on):

```
$ kubectl -n monitoring get prometheus kube-prometheus -o yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  # ...
  name: kube-prometheus
  namespace: monitoring
  # ...
serviceMonitorSelector:
  matchExpressions:
  - key: app
    operator: In
    values:
    - alertmanager
    - exporter-coredns
    - exporter-kube-controller-manager
    - exporter-kube-dns
    - exporter-kube-etcd
    - exporter-kube-scheduler
    - exporter-kube-state
    - exporter-kubelets
    - exporter-kubernetes
    - exporter-node
    - grafana
    - prometheus
    - prometheus-operator
```

The `elixir-drip-monitor` we've just defined will search for endpoints with the `app=elixir-drip` label in the `production` namespace, and then configure Prometheus to scrape metrics through the `web` port of those endpoints. We didn't specify a custom metrics path since, by default, ServiceMonitor gets its metrics through the `/metrics` path:

```
$ kubectl create -f monitoring/elixir-drip-service-monitor-prod.yml
servicemonitor.monitoring.coreos.com "elixir-drip-monitor" created

$ kubectl -n monitoring get servicemonitors
NAME AGE
elixir-drip-monitor 16s
# ...
```

After creating `elixir-drip-monitor`, we are able to find an `elixir-drip-loadbalancer` entry among the other Prometheus targets exposed by the http://localhost:49090/targets URL:

![](img/c636f4cfb07f25f1db535995e50706ed_523_0.png)

This means we can now query the ElixirDrip metrics with Prometheus. Despite Prometheus being able to graph our metrics, we'll use Grafana, an open source data visualization tool, which is able to use Prometheus as a data source. Luckily, the `prometheus-operator` we previously deployed also deploys a `grafana` pod, so it's just a matter of exposing port 3000 of the `kube-prometheus-grafana-6c9496d766-q7278` pod:

```
$ kubectl -n monitoring get pods -l app=kube-prometheus-grafana
NAME READY STATUS RESTARTS AGE
kube-prometheus-grafana-6c9496d766-q7278 2/2 Running 4 5d

$ kubectl -n monitoring port-forward kube-prometheus-grafana-6c9496d766-q7278 43000:3000
Forwarding from 127.0.0.1:43000 -> 3000
Forwarding from [::1]:43000 -> 3000
```

If you now go to `http://localhost:43000`, you will be greeted by the Grafana home page. You can already find some Kubernetes dashboards if you click on **Home** and then open the **General** folder. This is the aspect of the **Kubernetes Capacity Planning** dashboard:

![](img/c636f4cfb07f25f1db535995e50706ed_524_0.png)

Sign in as `admin/admin` in the bottom-left corner so we can create our very first dashboard. By clicking on the plus sign and selecting **Create dashboard**, we can create our ElixirDrip dashboard. Before adding any panels, save it by pressing `Ctrl + S` after giving it a name.

We'll now add a panel that will show the response times histogram of the `/health` endpoint used by Kubernetes to assess whether our ElixirDrip pods are working as expected. Inside the **New panel** section, select `Graph` and a new graph with dummy data (actually a random walk) will appear:

![](img/c636f4cfb07f25f1db535995e50706ed_525_0.png)

By clicking on the panel title, we can edit it. It's here that we can change the panel title (see the **General** tab), and it's also here that we'll configure everything related to our graph. Let's start with the `Metrics` tab and select the relevant metrics.

Because we used `prometheus-operator`, Grafana already has Prometheus configured as a data source, so it's just a matter of selecting it. If you start typing the metric name, Grafana will suggest metrics based on the available Prometheus data. Type `http_request_duration_microseconds_bucket{request_path="/health"}` as the first metric, and the graph will show you the `http_request_duration_microseconds_bucket` metric for every observation that has the custom label `request_path="/health"`.

By default, each legend key will show the full set of labels, including the `pod` and `le` labels (the latter gives you the metric bucket, a short form of *lower than or equal to*). To simplify, you can set each legend to only have the `pod` and `le` labels by writing the `{{pod}}` `{{le}}` expression in the `Legend format` field. However, we will still see 60 data series, because we currently have 20 buckets and three ElixirDrip pods (almost half of the data series don't even appear in the following screenshot):

![](img/c636f4cfb07f25f1db535995e50706ed_526_0.png)

If we don't care about specific pod response times, we may average the value of each bucket by using the `avg` aggregate operator:

```
avg(http_request_duration_microseconds_bucket{request_path="/health"}) by (le)
```

With this approach, we don't want to know which pod emitted the data point, so we should remove the `{{pod}}` part from the legend format. Because the HTTP response times are being measured in microseconds, let's add the µs units to the legend (`{{le}}` µs).

Let's also change the `Left Y axis` by setting `No. of requests` as its label and its unit as `none`, so we can see the complete legend values.

To improve the readability of our graph, change the draw mode of the graph to points in the Display tab:

![](img/c636f4cfb07f25f1db535995e50706ed_527_0.png)

By looking at a specific point in time in our graph, we can conclude that most of the observations belong to the +Inf, 10 seconds (10000000), 5 seconds (5000000), 2.5 seconds (2500000), 1 second (1000000), and 0.5 seconds (500000) buckets. So, let's just consider these buckets by selecting them with the following metric expression:

```
avg(http_request_duration_microseconds_bucket{
request_path="/health",
le=~".+Inf|10000000|5000000|2500000|1000000|500000"}) by (le)
```

We now have a tidier graph, showing us the /health average response time split between our six buckets:

Still, this graph doesn't tell us at a glance what's happening with our application. Let's try to see the whole picture with percentiles.

## Calculating percentiles

Instead of looking at the frequency of observations by bucket, let's use the `histogram_quantile()` query function to calculate some percentiles. This way, we'll better understand the distribution of our `/health` response times. We'll start by calculating the 50th percentile, replacing the previous metric with the following expression:

```
histogram_quantile(0.5,
    rate(
        http_request_duration_microseconds_bucket{request_path="/health",
        pod=~"elixir-drip.*"}[5m]
    ))
```

We are using the `rate(...) [5m]` function to take the 5-minute average rate of the samples in each histogram bucket. Because the histogram quantile is calculated for each combination of labels the metric has, we see three different graphs for each of our pods. We also need to change the `Legend format` to `50th percentile {{pod}}` and the unit of the `Left Y axis` to `microseconds (µs)`.

We can now observe that 50% of the `/health` requests in this time period took less than 8 seconds and the 8lts4 pod was the fastest pod:

![](img/c636f4cfb07f25f1db535995e50706ed_528_0.png)

Having three graphs for each percentile we draw will soon get messy, so let's aggregate this percentile to only have a single data series:

```
histogram_quantile(0.5,
    sum without(instance, pod, host) (
        rate(http_request_duration_microseconds_bucket{request_path="/health",
        pod=~"elixir-drip.*"}[5m])
    )
```

We are using sum without(...) to remove the labels that vary from pod to pod, so that the pod aggregation works as expected. However, with this approach we end up losing the information about each individual pod. What we would like is to have a way to easily select the value of the pod label expression. Fortunately, Grafana allows us to define variables that can then be used in our graphs.

## Setting Grafana variables

If you go to Settings (in the upper-right corner) and select Variables, we can add a new pod variable:

![](img/c636f4cfb07f25f1db535995e50706ed_529_0.png)

Besides the name of the variable, we set its type as query to obtain its values from a Prometheus http_request_duration_microseconds_bucket query. We then set /.*pod="([^"]*).*/ as the regex that will extract the pod label value from each observation.
