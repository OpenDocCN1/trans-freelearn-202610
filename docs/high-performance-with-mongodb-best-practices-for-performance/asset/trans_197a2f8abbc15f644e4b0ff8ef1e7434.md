
对于 Node.js 应用，实现 MongoDB 的 OpenTelemetry 插桩可能如下所示：

```js
// tracing.js
const opentelemetry = require('@opentelemetry/api');
const { NodeTracerProvider } = require('@opentelemetry/sdk-trace-node');
const { MongoDBInstrumentation } = require('@opentelemetry/instrumentation-mongodb');
const { registerInstrumentations } = require('@opentelemetry/instrumentation');

// Configure the tracer provider
const provider = new NodeTracerProvider();
provider.register();

// Register MongoDB instrumentation
registerInstrumentations({
  instrumentations: [new MongoDBInstrumentation()],
});

// Export traces using your preferred exporter
// (OTLP, Jaeger, Zipkin, etc.)
```

在此插桩就位后，MongoDB 操作会被自动捕获，并可发送到任何 OpenTelemetry 兼容的后端。

![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_350_0.png)

Figure 14.2: OpenTelemetry 捕获的 MongoDB 操作 span 可视化

上述截图来自可在 https://github.com/open-telemetry/opentelemetry-js-contrib/tree/main/examples/mongodb 获取的 OpenTelemetry MongoDB 插桩示例。

此插桩为每个 MongoDB 操作创建详细的 span，捕获操作类型、数据库和集合名称以及执行时间等关键信息，从而实现整个应用栈的精确性能监控和故障排查。

## 外部工具的注意事项

将 MongoDB 与外部监控系统集成时，应考虑多个重要因素以确保监控有效且安全。对 MongoDB 实例的访问以及流向监控系统的数据进行保护至关重要。这涉及实施强大的身份验证机制、传输中数据加密，并遵循监控用户的最小权限原则。仔细关注这些安全方面有助于保护您的数据并维护监控设置的完整性。

关键安全实践包括以下内容：

- **专用监控用户**：为监控工具专门创建 MongoDB 用户，并授予最低所需权限（通常为 clusterMonitor 角色）
- **强身份验证**：使用强密码，并在支持的情况下考虑基于证书的身份验证
- **TLS/SSL**：加密监控工具与 MongoDB 之间的通信
- **网络安全**：通过适当的防火墙规则限制监控工具对 MongoDB 的访问

除安全之外，集成外部监控工具的另一个关键方面是管理其资源消耗。监控系统本质上会与您的 MongoDB 部署交互以收集数据。如果配置不当，这种交互会给数据库服务器带来额外负载，可能影响其性能。因此，必须注意这些工具的资源消耗方式，以确保监控行为不会降低您试图观察的系统性能。

管理资源消耗的关键考虑因素包括以下内容：

- **抓取/轮询频率**：配置适当的指标收集间隔（通常为 10–60 秒），以在及时获取数据和最小化开销之间取得平衡
- **资源开销**：监控监控代理本身的资源消耗，以确保它们不会影响 MongoDB 性能
- **负载大小：** 注意收集的数据量，尤其是来自返回大型文档的命令（如 serverStatus）
- **在预发布环境中基准测试：** 在部署到生产环境之前，先在非生产环境中测试监控配置

总之，将外部监控工具与 MongoDB 集成需要平衡的方法，同时优先考虑安全性和性能。通过保护访问并仔细管理资源使用，您可以确保可靠的监控，而不会损害部署的稳定性和完整性。

## 常见性能模式及需要监控的内容

了解 MongoDB 中常见的性能瓶颈以及哪些指标会发出信号，对于主动性能管理至关重要。通过监控关键指标，您可以在问题对应用程序响应能力和可靠性造成重大影响之前检测和诊断问题。本节探讨了反复出现的性能挑战和需要跟踪的基本信号。

### 磁盘 I/O 瓶颈

存储子系统经常是数据库性能的限制因素，尤其是在重读或写负载下。当磁盘无法跟上 MongoDB 的需求时，操作会变慢，因为它们等待数据从磁盘读取或写入磁盘。

为了识别和诊断磁盘 I/O 瓶颈，监控几个关键指标至关重要：

- **磁盘延迟：** 测量磁盘级别读写操作完成所需的时间。持续较高的延迟（通常高于几毫秒，具体取决于存储介质）是 I/O 瓶颈的主要指标。这通常通过操作系统级工具或云提供商指标进行监控，Atlas 在其指标视图中展示磁盘延迟。
- **I/O 队列深度：** 跟踪等待磁盘处理的挂起 I/O 操作数量。持续较高的队列深度表示存储系统已饱和。
- **磁盘吞吐量（字节/秒）和 IOPS：** 监控实际数据传输速率和每秒 I/O 操作数。将这些指标与存储硬件的预配限制进行比较。达到这些限制可确认 I/O 瓶颈。

解决磁盘 I/O 瓶颈可能涉及优化查询以减少磁盘访问、改进索引、增加预配的 IOPS/吞吐量、升级存储硬件，或确保工作集适合内存以最大限度减少磁盘读取。

### 缓存压力（WiredTiger 缓存利用率）

MongoDB 的 WiredTiger 存储引擎使用内部缓存将频繁访问的数据和索引保留在内存中，以最大限度减少对较慢磁盘访问的需求。当活动工作集（当前工作负载所需的数据和索引）超过配置的 WiredTiger 缓存大小时，就会发生缓存压力。这会导致缓存驱逐活动增加和页面错误，从而降低性能。

若干关键指标可以帮助您检测和诊断 MongoDB 中 WiredTiger 缓存压力。监控这些指标可提供由内存限制引起的性能瓶颈的早期预警信号：

- **缓存利用率：** 跟踪当前使用的 WiredTiger 缓存量（serverStatus().wiredTiger.cache["bytes currently in the cache"]）。虽然高利用率通常是好的，但持续达到最大限制表明存在压力。
- **缓存驱逐率：** 监控页面从缓存中被驱逐以腾出空间供新数据的速率（serverStatus().wiredTiger.cache["unmodified pages evicted"]）。高或快速增加的驱逐率表明缓存频繁抖动。Atlas 通常可视化缓存活动，突出显示驱逐情况。
- **读入缓存的页面：** 跟踪数据从磁盘读入缓存的速率（serverStatus().wiredTiger.cache["pages read into cache"]）。高速率通常与高驱逐率相关，表明所需数据经常不在内存中找到。
- **页面错误：** 虽然并非完全与 WiredTiger 缓存相关（也可能发生操作系统页面错误），但频繁的页面错误（serverStatus().extra_info.page_faults 或操作系统指标）通常伴随缓存压力，表明 MongoDB 进程请求的数据当前不在物理 RAM 中。

监控这些指标可清晰了解缓存健康状况。高利用率本身并不坏，但当与高驱逐率、从磁盘读取页面增加以及页面错误相结合时，则表明缓存难以跟上工作负载，迫使频繁从较慢的磁盘存储中读取。
