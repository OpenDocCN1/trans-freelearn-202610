
- `scrape_*`：抓取指标反映了通过 OTEL 收集的持续时间和样本，这些收集是我们在 ECS 任务定义中选择的配置（/etc/ecs/ecs-amp-prometheus.yaml）的一部分：

![](img/ecd265dfca9bb64161408ba8a944a4d9_210_0.png)

图 8.7 – 不同类别指标的 Prometheus 选择器

一旦你按需选择了指标，就需要对 Web 应用施加负载，以便在各个平台和工具中看到相关的遥测数据。

## 对应用进行负载测试以生成数据

对 To-Do List Manager 应用进行负载测试的一个简单方法，是在你的 Cloud9 IDE 终端中运行一条针对应用负载均衡器 URL 的 curl 查询。

为了生成哈希，我们可以利用 `RANDOM` 变量，通过以下命令从中派生一个 md5 校验和值：

```
echo $RANDOM | md5sum | head -c 20
```

我们可以将哈希生成用于一条 curl 命令中，该命令在应用端点上触发 POST 请求：

```
curl --location --request POST 'http://chapt-farga-65sza285ppol-1511829492.eu-central-1.elb.amazonaws.com/' \
  --form "todo=`echo $RANDOM | md5sum | head -c 20`" \
  --form 'tasktype="Un-important"'
```

为了使其无限执行（直到手动中断），我们可以用 while 循环包装该命令：

```
while true; do curl --location --request POST 'http://chapt-farga-65sza285ppol-1511829492.eu-central-1.elb.amazonaws.com/' --form "todo=`echo $RANDOM | md5sum | head -c 20`" --form 'tasktype="Un-important"'; done
```

让这段代码运行一段时间，以便在我们的可观测性工具中生成足够的数据。

## 观察数据以了解应用行为

调试运维事件通常需要关联多个指标的信息，并从底层数据中得出洞察。我们的测试应用栈会在不同工具中发布不同类型的数据。我们在 CloudWatch 中拥有基础设施指标和日志，此外还会在 Prometheus 中发布指标。让我们查看一下 CloudWatch，看看是否有在 Flask 装饰器函数中启用的日志记录所生成的日志。

### 使用 Amazon CloudWatch Logs 监控日志

要检查 CloudWatch Logs，你可以前往 CloudWatch 服务控制台并点击相关的日志组。这种无缝的日志摄取由 ECS 与 CloudWatch 之间的集成提供。作为最佳实践，你应该始终将日志流输出到 STDOUT，以便数据能够轻松地被摄取到其他平台。

你可能还记得，我们在每个代表一个 ECS 容器的 CDK 构造中都有日志配置。对于应用容器，配置大致如下：

```
logging: ecs.LogDrivers.awsLogs({streamPrefix: "aws-devops-simplified-app-container"})
```

这一行配置会从应用容器摄取数据，并将其转发到 CloudWatch 服务，使用预定义的流前缀。在 CloudWatch 上检查此日志流的日志会显示一些有趣的洞察，如*图 8.8* 中突出显示的那样：

![](img/ecd265dfca9bb64161408ba8a944a4d9_212_0.png)

图 8.8 – 在 CloudWatch Logs 中捕获的应用日志

让我们仔细看看：

- 1. `GET /metrics`：来自 OpenTelemetry 收集器的 HTTP 调用会按预定义间隔抓取应用指标，并将其发布到 Prometheus 服务器。如我们所见，请求来源是 `127.0.0.1`，这符合预期，因为收集器与应用程序运行在同一个任务定义中。
- 2. `GET /`：这些是对应用根路径的调用，似乎来自两个 IP 地址：`10.0.3.223` 和 `10.0.2.30`。这些是应用负载均衡器的接口，它们会触发对应用的定期健康检查请求。
- 3. `POST`：这是对应用域的一个请求，是我们从本地终端触发以产生一些负载的 `curl` 命令的结果。你会看到很多这样的请求，具体取决于你让命令运行了多长时间。

CloudWatch 还允许你根据某些模式从这些日志中创建指标。因此，用户可以跟踪 1 分钟时间窗口内传入的 POST 调用数量，并在其超过预期阈值时发出警报。此外，你还可以跟踪此类请求的源 IP 地址，找出某个时间段内对应用负载贡献最大的来源。如你所见，可能性无穷无尽，这完全取决于你想从这些洞察中挖掘多深。

接下来，让我们看看我们的 Prometheus 指标。

### 使用 Amazon Managed Prometheus 对应用指标进行插桩

如前所述，在我们的 Flask 应用内实例化 Prometheus 客户端还会启用一个 `/metrics` 端点，将应用指标暴露给抓取器——在我们的案例中，就是 OTEL 收集器。

你也可以通过访问应用负载均衡器 URL（末尾加上 `/metrics` 后缀）来查看这些原始指标。

就我而言，访问 `http://chapt-farga-65sza285ppol-1511829492.eu-central-1.elb.amazonaws.com/metrics` URL 得到了以下输出：

```
...
# HELP flask_exporter_info Information about the Prometheus Flask exporter
# TYPE flask_exporter_info gauge
flask_exporter_info{version="0.22.3"} 1.0
...
# HELP flask_http_request_duration_seconds Flask HTTP request duration in seconds
# TYPE flask_http_request_duration_seconds histogram
flask_http_request_duration_seconds_bucket{le="0.005",method="GET",path="/",status="200"} 309.0
...
```

以 `# HELP` 开头的行描述了后续几行的相关信息以及如何解读它们，而以 `# TYPE` 开头的行则显示指标类型（直方图/仪表）和相关值，这些值随后可通过 Grafana 等工具进行可视化。

### 使用 Amazon Grafana 可视化数据

到目前为止，我们已经验证了应用在 `/metrics` 端点上暴露指标数据，OTEL 收集器正在抓取该端点，并将其进一步发布到我们在 CDK 栈中配置的 Prometheus 工作区中。我们架构中的最后一个工具是 Grafana。如*图 8.7* 所述，你可以选择某些指标显示在 Grafana 仪表板上。我选择了其中几个后得到了以下截图（*图 8.9*），它突出显示了后台运行 `curl` 命令期间持续一段时间的网络请求峰值：

![](img/ecd265dfca9bb64161408ba8a944a4d9_214_0.png)

图 8.9 – 在 Amazon Grafana 仪表板上可视化指标

理想情况下，你会根据定义系统行为的关键指标自定义字段选择器，并将其固定到一个可持续监控的仪表板上。

## 总结

开发软件应用只是挑战的一部分。在（企业规模的）生产环境中维护它们需要同样的投入和工程努力。这只能通过可靠的可观测性战略来实现，它不仅让你能更快地响应运维问题，还能收集有意义的洞察，从而让你持续为用户交付差异化的业务成果。

本章开始时，我们基本了解了什么是可观测性以及它为何重要。带着这些知识，我们接着讨论了 AWS 可观测性平台——CloudWatch——以及它为每个可观测性支柱（指标、日志和追踪）提供的解决方案或集成。此外，我们还讨论了 AWS 在 CNCF 的 OpenTelemetry 项目和 ADOT 收集器方面所做的努力，后者使你能无缝地收集、处理和导出指标、日志和追踪到其他 AWS 服务。

在 AWS 之外，Prometheus 和 Grafana 等解决方案因其成熟度、易用性和可扩展性而被企业广泛采用。为了确保涵盖所有这些解决方案的动手体验，我们继续为上一章中的测试应用 To-Do List Manager 部署了一个可观测性栈。这让我们能够全面了解如何集成所有这些工具，并使它们适应我们的特定需求。
