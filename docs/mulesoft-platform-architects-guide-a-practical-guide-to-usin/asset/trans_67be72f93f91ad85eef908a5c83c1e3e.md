
*   `Half-open`（半开）：行程超时过后，`Subscriber source`（订阅者源）会进入 `Half-open`（半开）状态。在下一次轮询消息时，`Subscriber source`（订阅者源）会从服务中检索一条消息，并使用该消息检查系统是否已恢复，然后再返回正常的 `Closed`（关闭）状态。

下图展示了 `Circuit Breaker pattern`（熔断器模式）的架构。

![Figure 7.9 - The Circuit Breaker pattern](image/B19922_07_09.jpg)

Figure 7.9 - The Circuit Breaker pattern

`MuleSoft` 允许我们使用 `Anypoint MQ` 或将 `Circuit Breaker policy`（熔断器策略）应用于 API 来实现 `Circuit Breaker pattern`（熔断器模式）。该策略可从 `MuleSoft Catalyst` 下载并发布到您组织的 `Anypoint exchange`（Anypoint Exchange）中。

# Anypoint MQ

`Anypoint MQ` 是由 `MuleSoft` 提供的一种多租户、基于云的托管队列服务，用于在应用程序之间异步交换数据。`Anypoint MQ broker`（Anypoint MQ 代理）允许应用程序将消息发布到队列，以便另一个应用程序可以从队列中消费该消息。`Anypoint MQ` 与 `Anypoint platform`（Anypoint 平台）完全集成，提供访问控制、客户端管理和连接器。

`Mule` 应用程序可以使用 `Anypoint MQ Connector`（Anypoint MQ 连接器）向队列发布和消费消息，而非 `Mule` 应用程序可以使用 `Anypoint MQ REST APIs`（Anypoint MQ REST API）向队列发布和消费消息。

## Message exchanges and queues（消息交换与队列）

`Message queues`（消息队列）是发布者可以发布消息、消费者可以消费消息的临时存储。`Message queues`（消息队列）可在多个应用程序之间实现可靠且可扩展的通信。

![Figure 7.10 - Anypoint MQ (queue)](image/B19922_07_10.jpg)

Figure 7.10 - Anypoint MQ (queue)

`Message exchanges`（消息交换）允许您将多个队列绑定在一起，以将相同的消息副本发送给绑定到该交换的所有队列。下图说明了消息如何从交换流向所有关联的队列。

![Figure 7.11 - Anypoint MQ (exchange)](image/B19922_07_11.jpg)

Figure 7.11 - Anypoint MQ (exchange)

通过交换，消息可利用消息路由规则智能地路由到特定队列。下图说明了消息如何根据路由规则流向特定队列。

![Figure 7.12 - Anypoint MQ (exchange intelligent routing)](image/B19922_07_12.jpg)

Figure 7.12 - Anypoint MQ (exchange intelligent routing)

使用消息交换时，请记住以下几点注意事项：

*   最多可将 450 个队列绑定到消息交换
*   仅可将 10 个队列绑定到启用了智能路由的消息交换
*   一个队列可绑定到多个队列

`Anypoint MQ` 支持 `FIFO` 队列。这些队列可保证消息的有序投递。对于要求**精确一次**投递和精确消息排序，但不要求快速投递速度的应用程序，`FIFO` 队列最为适用。

`Anypoint MQ` 支持 `FIFO` 队列中每个消息组 10 条在途消息。`Anypoint MQ` 中 `FIFO` 队列的消息组数量没有上限。

`Anypoint MQ` 支持的功能如下：

*   标准队列最多支持 120,000 条在途消息，`FIFO` 队列支持 10 条在途消息
*   `Anypoint MQ` 可在美国、欧洲和亚太地区的多个区域中进行配置
*   支持对队列中存储的消息进行加密

## Cross-region failover for Anypoint MQ standard queues（Anypoint MQ 标准队列的跨区域故障转移）

当您为队列启用故障转移时，`Anypoint MQ` 会自动在备用区域中创建一个回退队列。如果 `Mule` 应用程序无法连接到该区域的 `Anypoint MQ` 服务器，它将从回退队列发布和消费消息。

一旦连接器确定主区域不可用，它就会切换到从回退队列发布和消费新消息，并定期验证主区域是否恢复运行。

跨区域故障转移的注意事项如下：

*   故障转移支持标准队列，但不支持 `FIFO` 队列和消息交换。
*   `Anypoint government cloud`（Anypoint 政府云）不支持故障转移，因为它仅支持单一区域。`Anypoint government cloud`（Anypoint 政府云）将在 [*Chapter 13*](#B19922_13.xhtml#_idTextAnchor349) 中讨论。
*   故障转移支持需要 `Anypoint MQ Connector 4.0.5` 或更高版本。
*   在某些国家，`Anypoint MQ` 仅在一个区域可用。如果这些区域的队列启用了故障转移且主区域宕机，数据将存储在该国家以外的区域中，直到主区域恢复可用。
*   故障转移默认未启用。一旦启用故障转移，它将在故障转移区域中创建一个带有 `_fb` 后缀的队列。例如，如果您在主区域有一个名为 `myOrderQueue` 的队列，它将在故障转移区域中创建 `myOrderQueue_fb`。

## Dead-letter queues（死信队列）

您可以使用 `Anypoint MQ` 确保无法投递的消息被路由到 `DLQ`。之后，您可以检查发送到 `DLQ` 的消息，以确定它们未被投递的原因。`DLQ` 仅接收未投递的消息；除此之外，其功能类似于任何其他队列。创建队列时，您可以设置投递延迟、加密和 `TTL` 值。

`Anypoint MQ` 提供的其他功能包括：

*   提供开箱即用的 `Anypoint MQ Connector`，用于通过消息交换和队列发布和消费消息。
*   非 `MuleSoft` 应用程序或第三方应用程序可使用 `Anypoint MQ REST APIs` 从 `Anypoint MQ` 发布和消费消息。
*   可传输高达 10 MB 的大量负载。如果负载包含文本以外的任何格式（如 CSV、HTML、JSON 和 XML），`Anypoint MQ` 会在发送前将负载转换为字符串，这会增加负载的大小。如果因这种转换导致负载大于 10 MB 的最大负载大小，可能会发生负载过大错误。

## The Circuit Breaker pattern with Anypoint MQ（Anypoint MQ 的熔断器模式）

`Subscriber source`（订阅者源）提供 `Circuit Breaker`（熔断器）功能，允许您控制连接器在处理消费消息时出现错误时的响应方式。您可以使用 `Circuit Breaker`（熔断器）来处理您连接的外部服务的任何中断。`Circuit Breaker`（熔断器）允许外部服务在较低负载下恢复，并允许系统停止发出请求。下图显示了在 `Anypoint MQ` 上配置 `Circuit Breaker pattern`（熔断器模式）的情况。

![Figure 7.13 - The Circuit Breaker pattern with Anypoint MQ](image/B19922_07_13.jpg)

Figure 7.13 - The Circuit Breaker pattern with Anypoint MQ

在 `Anypoint MQ` 上启用 `Circuit Breaker pattern`（熔断器模式）需要配置以下参数：

*   `onErrorType`：这决定了在执行流程时哪些类型的错误被视为失败。仅当流程以错误传播结束时，错误发生才会被计数。每个错误都会自动被解释为电路故障。
*   `errorsThreshold`：这决定了 `Circuit Breaker`（熔断器）必须遇到多少个 `onErrorType` 错误才会打开。
*   `tripTimeout`：如果达到 `errorsThreshold`，电路将保持打开状态一段时间，可用此值指定。
*   `circuitName`：将绑定到此规范的 `Circuit Breaker`（熔断器）的名称。默认情况下，每个队列都有自己的 `Circuit Breakers`（熔断器）。

# Event-driven architecture (EDA)（事件驱动架构 (EDA)）

采用微服务构建的现代应用程序经常使用 `EDA`，它利用事件在解耦服务之间触发和通信。事件是状态的更新或更改，例如在网上商店将产品添加到购物车时。

现在让我们讨论 `EDA` 中的解耦和松耦合。
