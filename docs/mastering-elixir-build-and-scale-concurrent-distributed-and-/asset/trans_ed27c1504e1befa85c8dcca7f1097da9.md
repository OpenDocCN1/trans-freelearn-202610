
我们正在自定义 ServiceMonitor 中设置 `app=prometheus` 标签，因为 `kube-prometheus` 资源（类型为 `Prometheus`，也是 Prometheus operator 引入的自定义资源）默认会搜索具有特定 `app` 标签值的 ServiceMonitor（`alertmanager`、`grafana`、`prometheus` 等）：

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

我们刚刚定义的 `elixir-drip-monitor` 将在 `production` 命名空间中搜索带有 `app=elixir-drip` 标签的端点，然后配置 Prometheus 通过这些端点的 `web` 端口抓取指标。我们没有指定自定义指标路径，因为默认情况下 ServiceMonitor 通过 `/metrics` 路径获取指标：

```
$ kubectl create -f monitoring/elixir-drip-service-monitor-prod.yml
servicemonitor.monitoring.coreos.com "elixir-drip-monitor" created

$ kubectl -n monitoring get servicemonitors
NAME AGE
elixir-drip-monitor 16s
# ...
```

创建 `elixir-drip-monitor` 后，我们能够在 http://localhost:49090/targets URL 暴露的其他 Prometheus 目标中看到 `elixir-drip-loadbalancer` 条目：

![](img/c636f4cfb07f25f1db535995e50706ed_523_0.png)

这意味着我们现在可以用 Prometheus 查询 ElixirDrip 指标。尽管 Prometheus 能够绘制我们的指标图表，但我们将使用 Grafana，这是一款开源数据可视化工具，能够使用 Prometheus 作为数据源。幸运的是，我们之前部署的 `prometheus-operator` 也部署了一个 `grafana` Pod，因此只需将 `kube-prometheus-grafana-6c9496d766-q7278` Pod 的 3000 端口暴露出来即可：

```
$ kubectl -n monitoring get pods -l app=kube-prometheus-grafana
NAME READY STATUS RESTARTS AGE
kube-prometheus-grafana-6c9496d766-q7278 2/2 Running 4 5d

$ kubectl -n monitoring port-forward kube-prometheus-grafana-6c9496d766-q7278 43000:3000
Forwarding from 127.0.0.1:43000 -> 3000
Forwarding from [::1]:43000 -> 3000
```

如果你现在访问 `http://localhost:43000`，将会看到 Grafana 主页。如果你点击 **Home** 然后打开 **General** 文件夹，就能找到一些 Kubernetes 仪表盘。这就是 **Kubernetes Capacity Planning** 仪表盘的外观：

![](img/c636f4cfb07f25f1db535995e50706ed_524_0.png)

在左下角使用 `admin/admin` 登录，以便我们可以创建我们的第一个仪表盘。通过点击加号并选择 **Create dashboard**，我们可以创建 ElixirDrip 仪表盘。在添加任何面板之前，先为其命名，然后按 `Ctrl + S` 保存。

我们现在将添加一个面板，显示 Kubernetes 用于评估我们的 ElixirDrip Pod 是否按预期工作 的 `/health` 端点的响应时间直方图。在 **New panel** 部分中，选择 `Graph`，一个带有虚拟数据（实际上是随机游走）的新图表将会出现：

![](img/c636f4cfb07f25f1db535995e50706ed_525_0.png)

通过点击面板标题，我们可以编辑它。在这里我们可以更改面板标题（参见 **General** 选项卡），也是在这里我们将配置与图表相关的所有内容。让我们从 `Metrics` 选项卡开始并选择相关指标。

因为我们使用了 `prometheus-operator`，Grafana 已经将 Prometheus 配置为数据源，只需选择它即可。如果你开始输入指标名称，Grafana 会根据可用的 Prometheus 数据建议指标。输入 `http_request_duration_microseconds_bucket{request_path="/health"}` 作为第一个指标，图表将显示具有自定义标签 `request_path="/health"` 的每个观测值的 `http_request_duration_microseconds_bucket` 指标。

默认情况下，每个图例键都会显示完整的标签集，包括 `pod` 和 `le` 标签（后者给出指标桶，是 *lower than or equal to* 的简写）。为了简化，你可以在 `Legend format` 字段中写入 `{{pod}}` `{{le}}` 表达式，使每个图例仅包含 `pod` 和 `le` 标签。然而，我们仍然会看到 60 个数据系列，因为我们目前有 20 个桶和三个 ElixirDrip Pod（几乎一半的数据系列甚至没有出现在下面的截图中）：

![](img/c636f4cfb07f25f1db535995e50706ed_526_0.png)

如果我们不关心特定 Pod 的响应时间，可以使用 `avg` 聚合操作符对每个桶的值进行平均：

```
avg(http_request_duration_microseconds_bucket{request_path="/health"}) by (le)
```

采用这种方法，我们不想知道数据点是由哪个 Pod 发出的，因此应从图例格式中删除 `{{pod}}` 部分。由于 HTTP 响应时间以微秒为单位测量，让我们在图例中添加 µs 单位（`{{le}}` µs）。

让我们还更改 `Left Y axis`，将其标签设置为 `No. of requests`，单位设置为 `none`，以便可以看到完整的图例值。

为了提高图表的可读性，在 Display 选项卡中将图表的绘制模式更改为点：

![](img/c636f4cfb07f25f1db535995e50706ed_527_0.png)

通过查看图表中某个特定时间点，我们可以得出大多数观测值属于 +Inf、10 秒（10000000）、5 秒（5000000）、2.5 秒（2500000）、1 秒（1000000）和 0.5 秒（500000）桶。因此，让我们只考虑这些桶，使用以下指标表达式选择它们：

```
avg(http_request_duration_microseconds_bucket{
request_path="/health",
le=~".+Inf|10000000|5000000|2500000|1000000|500000"}) by (le)
```

我们现在有一个更整洁的图表，显示了 /health 平均响应时间在我们六个桶之间的分布：

尽管如此，这个图表仍然无法一目了然地告诉我们应用程序发生了什么。让我们尝试使用百分位数来全面了解情况。

## 计算百分位数

而不是查看按桶划分的观测频率，让我们使用 `histogram_quantile()` 查询函数来计算一些百分位数。这样，我们就能更好地了解 `/health` 响应时间的分布。我们将从计算第 50 百分位数开始，用以下表达式替换之前的指标：

```
histogram_quantile(0.5,
    rate(
        http_request_duration_microseconds_bucket{request_path="/health",
        pod=~"elixir-drip.*"}[5m]
    ))
```

我们使用 `rate(...) [5m]` 函数来获取每个直方图桶中样本的 5 分钟平均速率。因为直方图分位数是针对指标拥有的每个标签组合计算的，所以我们可以看到每个 Pod 的三个不同图表。我们还需要将 `Legend format` 更改为 `50th percentile {{pod}}`，并将 `Left Y axis` 的单位更改为 `microseconds (µs)`。

我们现在可以观察到，在此时间段内，50% 的 `/health` 请求耗时少于 8 秒，且 8lts4 Pod 是最快的 Pod：

![](img/c636f4cfb07f25f1db535995e50706ed_528_0.png)

随着我们绘制的每个百分位数都有三个图表，很快就会变得混乱，因此让我们将此百分位数聚合为仅有一个数据系列：

```
histogram_quantile(0.5,
    sum without(instance, pod, host) (
        rate(http_request_duration_microseconds_bucket{request_path="/health",
        pod=~"elixir-drip.*"}[5m])
    )
```

我们使用 sum without(...) 来删除 Pod 之间不同的标签，以便 Pod 聚合按预期工作。然而，采用这种方法我们最终会丢失每个单独 Pod 的信息。我们希望能够轻松选择 Pod 标签表达式的值。幸运的是，Grafana 允许我们定义变量，然后可以在图表中使用这些变量。

## 设置 Grafana 变量

如果你转到设置（右上角）并选择 Variables，我们可以添加一个新的 Pod 变量：

![](img/c636f4cfb07f25f1db535995e50706ed_529_0.png)

除了变量名称外，我们将其类型设置为 query，以从 Prometheus http_request_duration_microseconds_bucket 查询中获取其值。然后我们设置 /.*pod="([^"]*).*/ 作为正则表达式，从每个观测值中提取 Pod 标签值。
