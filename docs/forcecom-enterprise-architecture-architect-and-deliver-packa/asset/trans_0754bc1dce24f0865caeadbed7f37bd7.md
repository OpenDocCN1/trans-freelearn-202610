

未处理的 Apex 异常、调控器异常或验证规则可能导致执行上下文中止。但是，在某些情况下，仅执行部分回滚可能是可取的。对于这些情况，平台提供了 **保存点**，可用于在执行上下文中放置标记，以记录在创建保存点时写入了哪些数据。之后，保存点可在执行中用作回滚位置，以响应错误处理或导致替代代码路径并导致数据库不同更新的条件，从而恢复进一步处理。

> 需要注意的是，使用 Apex `try` 和 `catch` 关键字捕获异常（例如，在 Visualforce 控制器方法中）会导致平台提交异常发生前写入的任何数据。这可能导致执行上下文中正在更新的对象之间出现部分更新的数据，从而给您的应用程序带来数据完整性问题。保存点可以通过在 `catch` 代码块中使用回滚来避免这种情况。但是，每次这样做对开发人员来说都会带来相当大的样板代码开销。本书后面将更详细地审查此做法，并介绍一种称为 **工作单元** 的模式，以帮助自动化此过程。

## Apex 调控器和命名空间

平台调控器防止任何单个执行上下文在服务上消耗过多资源，这可能对其用户有害。总体而言，执行上下文不能超过 10 分钟，尽管在实践中，在执行上下文内，其他限制可能会在此之前达到。

例如，在执行上下文中执行的 Apex 代码单元只能总共执行最多 10 或 60 秒，具体取决于上下文。多年来，Salesforce 一直致力于整合曾经令人困惑的一系列调控器，这些调控器也因多种 Apex 代码上下文而异。幸运的是，如今调控器更容易遵循，并且仅根据上下文是交互式还是批处理（异步）而有所不同。

## 命名空间和调控器作用域

对于打包解决方案，一个重要的考虑因素是调控器的作用域。换句话说，它是仅适用于打包代码，还是适用于在执行上下文中执行的所有代码？请记住，执行上下文可能包含订户组织中其他开发者或其他安装的打包解决方案的代码。

您的包命名空间充当一些调控器的容器，保护您的代码免受执行上下文中运行的其他打包代码（例如触发器）消耗限制。平台支持在一个执行上下文中拥有无限数量的命名空间作用域。对于未作为托管包一部分部署的代码（例如直接在订户组织中开发的代码），有一个默认命名空间。

在命名空间中运行的代码不能超过由命名空间限定的调控器（本章后面将重点介绍）。虽然执行上下文可以运行来自无限数量命名空间的代码，但会强制执行跨命名空间累积限制。这是每个命名空间特定限制的 11 倍。例如，在任何数量的命名空间中最多允许 1650 条 DML 语句（每个命名空间 150 条）。更多示例请参阅 Apex 开发者指南中“Execution Governors and Limits”页面下名为“Per-Transaction Certified Manage Package Limits”的子部分。

> 实际上，在我看来，150 对于单个命名空间来说是一个相当大的 DML 数量。如果您正在观察批量化，这将需要您在一个请求中更新 150 个不同的对象！如果是这种情况并且您正在遵循 DML 批量化，您可能需要审查对象模型设计，看看是否可以进一步反规范化。虽然未记录，但我怀疑 11 的累积乘数可能基于历史上 10 个命名空间限制加上默认命名空间。

当您在打包组织中观察调试日志时，可以看到命名空间发挥作用的证据。在每个 Apex 代码单元完成之前，请注意调试日志的 LIMIT_USAGE_FOR_NS 部分针对默认和 force 命名空间（您的命名空间将不同）重复出现，如下所示，允许您在整个执行上下文中审查调控器的增量用法：

```
13:47:43.712 (412396000)|CUMULATIVE_LIMIT_USAGE
13:47:43.712|LIMIT_USAGE_FOR_NS|(default)|
Number of SOQL queries: 0 out of 100
Number of query rows: 0 out of 50000
Number of SOSL queries: 0 out of 20
Number of DML statements: 0 out of 150
Number of DML rows: 0 out of 10000
Number of code statements: 0 out of 200000
Maximum CPU time: 0 out of 10000
Maximum heap size: 0 out of 6000000
Number of callouts: 0 out of 10
Number of Email Invocations: 0 out of 10
Number of fields describes: 0 out of 100
Number of record type describes: 0 out of 100
Number of child relationships describes: 0 out of 100
Number of picklist describes: 0 out of 100
Number of future calls: 0 out of 10
13:47:43.712|LIMIT_USAGE_FOR_NS|force|
Number of SOQL queries: 1 out of 100
Number of query rows: 1 out of 50000
Number of SOSL queries: 0 out of 20
Number of DML statements: 2 out of 150
Number of DML rows: 3 out of 10000
Number of code statements: 85 out of 200000
Maximum CPU time: 0 out of 10000
Maximum heap size: 0 out of 6000000
Number of callouts: 0 out of 10
Number of Email Invocations: 0 out of 10
Number of fields describes: 0 out of 100
Number of record type describes: 0 out of 100
Number of child relationships describes: 0 out of 100
Number of picklist describes: 0 out of 100
Number of future calls: 0 out of 10
13:47:43.712|CUMULATIVE_LIMIT_USAGE_END
13:47:43.412|CODE_UNIT_FINISHED|execute_anonymous_apex
```

请注意，无论作用域如何，汇总中都会显示所有调控器；例如，CPU 时间调控器是执行作用域调控器。这使您可以跟踪来自所有命名空间的 Apex 代码单元中 Apex CPU 时间的消耗情况。开发者控制台选项提供了一些出色的工具来分析调控器的消耗情况。

## 确定性和非确定性调控器

从测试和支持的角度考虑调控器时，您应该了解哪些调控器是确定性的，哪些不是。例如，在分析您的应用程序时，这是指能够从反复运行完全相同的测试的调试日志中获得完全相同的结果（例如前面的结果）。以下列出了两种类型调控器的示例：

- 确定性调控器的示例是 SOQL 和 DML 调控器。如果您重复相同的测试场景（在您的开发或打包组织中），您始终会得到相同的数据库查询和更新使用情况；因此，您可以可靠地确定这些方面的任何变化是否需要调查。

- 非确定性调控器的示例是 CPU 时间调控器。每次执行之间使用的时间量将根据 Salesforce 服务器在任何给定时间的负载而有所不同。因此，难以准确确定您的应用程序中的更改是否真的影响了这一点。话虽如此，它可以作为一个有用的指标，具体取决于您在软件构建之间看到的差异。但是，请记住，您捕获的基线信息会随着时间的推移变得越来越不准确，例如，在平台发布之间。

在 CPU 调控器中，平台将执行上下文允许花费在执行 Apex 代码上的时间限制为交互式上下文 10 秒，异步上下文 60 秒（例如，Batch Apex），尽管在实践中，平台似乎表现出一定的容忍度。相比之下，应用于一致测试场景时，SOQL 查询数或 DML 语句数始终是恒定的。

由于 CPU 调控器是非确定性的，测试时只应将其用作准则。我的建议是不要将 `Limits.getCPUlimit()` 方法用作应用程序中交互式和批处理处理之间的分支机制。如果您需要这种类型的分支，请专注于使用应用程序的某些方面，例如选择条件、选定的行数或订户组织中的某些其他可配置设置（可能通过自定义设置）。

您可能希望考虑在 Apex 测试上下文中测试某些调控器，以监控查询或 DML 消耗量的变化。此类测试可以为您提供效率低下已悄悄进入代码的早期警告。使用 `Limits` 类方法来捕获 SOQL 和/或 DML 调控器的前后状态。然后，应用预期使用情况的严格比较（例如，测试执行后预期 10 个查询）或加减容差以允许代码演进。

## Apex 包开发人员的关键调控器

下表是关键调控器、其作用域及其是否为确定性的摘要。请注意，Salesforce 不断监控其服务的使用情况并调整其调控器。请始终查看 *Apex 开发者指南* 中有关调控器的最新信息。

| 调控器 | 作用域 | 确定性 |
|---|---|---|
| 执行上下文超时 | 执行上下文 | 否 |
| Apex CPU 调控器 | 执行上下文 | 否 |
| SOQL 查询 | 命名空间 | 是 |
| DML 语句 | 命名空间 | 是 |
| 查询行数 | 命名空间 | 是 |

