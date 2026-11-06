
## Prometheus + Grafana

Prometheus 和 Grafana 共同构成了一个流行的开源监控解决方案。Prometheus 收集并存储指标，而 Grafana 通过可定制的仪表板对其进行可视化。MongoDB Atlas 提供了与该技术栈的原生集成，使您能够将 MongoDB 部署与其他基础设施组件一起监控。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_345_0.png)

> **注意**
> 
> Prometheus 集成仅在 M10 及以上集群上可用。

## Atlas integration for Prometheus

MongoDB Atlas 提供了与 Prometheus 的直接集成，无需第三方导出器。以下是设置方法：

- **配置身份验证：** 在 Atlas UI 中，导航至 **Integrations** 选项卡并设置专门用于指标收集的 Prometheus 凭据。
- **选择发现方法：** 根据您的基础设施和偏好，选择 **HTTP Service Discovery**（推荐）或 **File Service Discovery**。
- **更新 Prometheus 配置：** 在您的 Prometheus 服务器中添加适当的抓取配置。

综上所述，将 MongoDB Atlas 与 Prometheus 集成涉及三个关键步骤：

- 1. 在 Atlas UI 中配置身份验证凭据。
- 2. 选择服务发现方法（HTTP 或基于文件）。
- 3. 更新 Prometheus 服务器的配置以包含必要的抓取设置。

以下是 prometheus.yml 文件中抓取配置可能的样子示例：

```yaml
scrape_configs:
  - job_name: "<insert-job-name>"
    scrape_interval: 10s
    metrics_path: /metrics
    scheme: https
    basic_auth:
      username: <prometheus-username>
      password: <prometheus-password>
    http_sd_configs:
      - url: <url-of-the-service-discovery-configuration>
        refresh_interval: 60s
        basic_auth:
          username: <prometheus-username>
          password: <prometheus-password>
```

配置完成后，Prometheus 将自动发现并抓取所有 Atlas 集群的各种指标。这些指标包括操作速率（如查询、插入、更新和删除）、资源利用率（如 CPU、内存和连接数）、复制指标（包括延迟和 oplog 窗口）以及存储指标（如磁盘使用量和 IOPS）。

## Visualizing with Grafana

为了完善您的监控解决方案并深入了解部署的性能，您可以将预构建的 MongoDB 仪表板导入 Grafana：

- 1. 在 Grafana 中，导航至导入屏幕（点击 + 图标，然后选择导入）。
- 2. 上传示例仪表板 JSON 文件（例如 `mongo-metrics.json` 或 `hardware-metrics.json`）。这些通常可以在 Atlas UI 或官方文档中找到。
- 3. 确保导入的仪表板配置为使用您的 Prometheus 数据源。
- 4. 使用这些专用可视化开始监控您的 MongoDB 部署。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_346_0.png)

图 14.1：Grafana 中的示例 MongoDB 指标仪表板

有关详细的分步说明以及访问示例仪表板 JSON 文件，请参阅 https://www.mongodb.com/docs/atlas/tutorial/prometheus-integration/#import-a-sample-grafana-dashboard 处的官方 MongoDB Atlas 文档。

此集成适用于 MongoDB Atlas 以及 Cloud Manager 和 Ops Manager 部署，无论您的 MongoDB 基础设施是完全托管还是自托管，都能提供一致的监控能力。

## Datadog

Datadog 是一个全面的商业 SaaS 可观测性平台，提供基础设施监控、SAPM、日志管理等功能。MongoDB Atlas 提供了与 Datadog 的一流集成，可实现对数据库部署的无缝监控。

MongoDB Atlas 与 Datadog 的集成允许您直接在 Datadog 仪表板中监控 Atlas 集群，提供数据库性能与其他应用程序和基础设施指标的统一视图。

此集成的关键功能包括以下内容：

- **原生集成：** 只需在 Atlas UI 中点击几下即可配置 Atlas 与 Datadog 之间的连接
- **自动指标收集：** 连接后，Atlas 会自动将性能指标发送到您的 Datadog 账户
- **预构建仪表板：** 集成包含显示关键 MongoDB 性能指标的预配置仪表板
- **统一告警：** 在 Datadog 中基于 Atlas 指标创建告警，并与您现有的通知工作流集成
- **跨平台关联：** 轻松将数据库性能问题与应用程序和基础设施指标关联起来

> **注意**
> 
> Datadog 集成仅在 M10 及以上集群上可用。您需要一个 Datadog 账户和 API 密钥。

要设置集成，请按照以下步骤操作：

- 1. 确保您在 Atlas 中拥有 *Project Owner* 访问权限。
- 2. 在 Atlas 中，导航至您的项目，然后点击 **Integrations**。
- 3. 点击 **Datadog** 块。
- 4. 输入您的 Datadog API 密钥。您可以在 Datadog 账户的 **Integrations > APIs** 下找到或创建它。
- 5. 配置您希望 Datadog 监控的集群和特定指标。
- 6. 可选地，要跟踪高基数指标，请启用 `sendDatabaseMetrics` 和 `sendCollectionLatencyMetrics`。这可以通过 Atlas UI 或 Atlas Administration API 完成。
- 7. 配置完成后，您可以在 Datadog 中访问您的 MongoDB Atlas 仪表板和指标。

有关更详细的步骤和信息，请参阅 https://www.mongodb.com/docs/atlas/tutorial/datadog-integration/#procedure 处的官方 MongoDB Atlas Datadog 集成文档。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_348_0.png)

> **Atlas 项目指标自动发送到 Datadog**
> 
> 如果您配置 Atlas 项目将警报和事件发送到 Datadog，则无需单独执行此流程。Atlas 通过用于警报和事件的同一集成将项目指标发送到 Datadog。

集成完成后，您可以开始探索 Datadog 中的预构建仪表板，以了解 Atlas 集群的性能概况。考虑为关键指标（如 CPU 利用率、磁盘空间和复制延迟）设置警报，以便在出现潜在问题时收到通知。您还可以开始将 MongoDB 指标与应用程序和基础设施数据关联，以获得系统的整体视图。我们将在后续章节中更深入地探讨性能监控策略和告警最佳实践。

## Application performance monitoring platforms

**New Relic**、**AppDynamics** 和 **Dynatrace** 等平台专注于 **application performance monitoring (APM)**。它们的主要优势在于端到端跟踪应用程序请求，提供应用程序代码如何与数据库交互的深入可见性。

APM 代理通常通过特定语言的库集成到您的应用程序代码中。当您的应用程序进行数据库调用时，APM 代理会执行以下操作：

- 1. 拦截调用
- 2. 测量其持续时间
- 3. 捕获相关上下文（如查询本身，通常为规范化）
- 4. 将其与更广泛的请求上下文关联

此集成使 APM 平台能够执行以下操作：

- 从用户的浏览器或 API 客户端跟踪单个请求，经过多个服务直至特定的数据库查询
- 在完整用户事务的上下文中识别缓慢的数据库调用
- 将应用程序错误或延迟峰值与特定的数据库操作关联
- 提供聚合视图，显示数据库性能如何影响整体应用程序性能

总之，APM 平台提供了一个强大的视角来理解数据库交互如何融入应用程序行为的更大图景，使更易于确定影响最终用户体验的与数据库相关的瓶颈。

## OpenTelemetry

OpenTelemetry 是一个开源可观测性框架，提供厂商中立的 API、库和 instrumentation 来捕获分布式追踪、指标和日志。它代表了可观测性 instrumentation 的未来，并在行业中得到日益广泛的采用。

OpenTelemetry 为流行的编程语言和 MongoDB 驱动程序提供 instrumentation 库。通过将这些库纳入您的应用程序，MongoDB 驱动程序调用会自动作为分布式追踪中的“span”被捕获。每个 span 记录以下内容：

- 数据库操作的持续时间
- 操作类型（查询、更新、插入等）
- 目标集合或数据库
- 发生的任何错误

这些 spans 提供了每次数据库交互的详细记录，捕获执行了什么操作、花费了多长时间、在何处执行以及是否成功。这些细粒度数据对于理解数据库性能和诊断问题至关重要。
