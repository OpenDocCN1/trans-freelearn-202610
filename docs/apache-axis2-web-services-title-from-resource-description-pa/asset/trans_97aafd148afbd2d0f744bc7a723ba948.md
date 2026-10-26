

仓库目录由两个主要子目录组成，名为 `services` 和 `modules`。此外，你可能还有一个可选的子目录名为 `lib`。如果你想部署一个服务，你需要将服务归档文件放入 `services` 目录。类似地，如果你想部署一个模块，那么你需要将模块归档文件放入 `modules` 目录。`lib` 目录的想法是存储将要在服务和模块之间共享的第三方库。参考下一张截图以查看目录结构：

![](img/e2fb05de168053e6227dab034eff9353_92_0.png)

如果 `modules` 目录中的一个或多个模块需要共享某些资源，那么它们可以将这些资源添加到 `modules` 目录内的 `lib` 目录中。类似地，如果 `services` 目录中的服务想要共享一些通用资源，适当的位置是 `services` 目录内的 `lib` 目录。

## 处理程序（模块）部署方式的变更

**服务扩展**的概念是 Apache Axis 范式中的一个新功能，但开发者在 Axis 1.x 中通过艰难的工作实现了相同的目标。因此，想法是扩展系统的核心功能或提供服务质量。在 Axis 1.x 的情况下，如果你需要扩展其核心功能，你需要编写一个处理程序（执行链中的最小单元），更改全局配置文件以添加处理程序，最后重启系统。

## 全局描述符或 axis2.xml

如前所述，Axis2 中的配置可以使用 XML 描述符指定。这为扩展和更改 Axis2 提供了更大的灵活性。无需去更改代码以获得不同的配置。即使对于核心功能也是如此。如果你考虑 Axis2 的全局配置文件——axis2.xml——它包含了运行 Axis2 所需的所有最小配置。Axis2 的最小配置包括：

- 配置参数
- 传输发送器
- 传输监听器
- 执行链和阶段
- 默认调度器
- 默认消息接收器
- 默认客户端侧配置
- 全局模块
- WS-Policy（全局级别策略）

其中一些给定的术语对你来说可能不熟悉，但你现在无需担心。我们将在后续章节中讨论这些术语。Axis2 附带默认的 axis2.xml，它具有启动 Axis2 所需的最小配置。然而，你可以根据自己的意愿更改它，并使用自己的 axis2.xml 启动 Axis2。这里需要记住的关键点是，如果你对 axis2.xml 做了任何更改，你必须重启系统才能使这些更改生效。

## 服务描述符（services.xml）

正如我们在上一节讨论的，axis2.xml 指定了影响整个系统的配置。然而，services.xml 用于配置特定服务或服务组。在下一节中，我们将查看在 Axis2 中部署服务的可用方式。基于归档的部署和基于目录的部署可以被认为是两种最常用的技术。在这两种情况下，对于一个服务要成为有效服务，需要有一个 services.xml 文件。服务描述符用于指定以下类型的配置（其中一些是可选的）：

- 服务名称
- 服务的目标名称空间
- 会话作用域
- 暴露传输
- 服务级别和操作级别参数
- 消息接收器
- 服务级别模块
- 操作 – 暴露操作以及非暴露操作
- Bean 映射
- 对象提供者
- 服务和操作级别策略

我们将在第 7 章《编写 Axis2 服务》中更深入地学习 services.xml 以及如何编写和创建服务。

## 模块描述符或 module.xml

现在显而易见，module.xml 用于配置 Axis2 模块；因此它也有不同类型的配置：

- 处理程序及其阶段规则
- 模块参数
- 端点
- WS-Policy

在第 8 章《编写 Axis2 模块》中，我们将更深入地学习 Axis2 模块以及如何编写它们。

## 可用的部署选项

在 Axis2 的初始阶段，它仅具有基于归档的部署。然而，后来它引入了许多便捷的部署选项。这使得服务作者的工作变得更容易。全部加起来，Axis2 具有以下服务部署选项：

- 基于归档的部署
- 基于目录的部署
- 使用归档文件以编程方式部署
- 以编程方式将 Java 类制作成 Web 服务
- 纯 Java 对象（POJO）部署支持以及注解
- 一行代码部署并启动 Axis2

## 基于归档的部署

在 Axis2 中部署服务的**基于归档的部署**是最常见且推荐的方法。在这里，你可以获得许多配置选项，与其他类型相比更灵活。在本章中，我们将通过示例讨论更多关于基于归档的部署。

## 基于目录的部署

**基于目录的部署**几乎与基于归档的部署相同；唯一的区别是，与创建归档文件相比，你可以作为目录部署服务。目录的结构与归档文件相同。

## 以编程方式部署服务

以归档文件以编程方式部署服务并不是真正用户的需求；而是模块作者的需求，其中某些模块需要在运行时部署 Web 服务以提供该特定模块的完整功能。

要以编程方式创建服务（`ServiceGroup`），你需要有表示服务归档文件的 file 对象以及指向 Axis2 运行时或 `ConfigurationContext` 的指针。一旦你拥有这两个，我们就可以创建 Web 服务，如以下代码片段所示。这种方法的优点是你不需要将服务归档文件复制到仓库中，它仅在你的服务运行时可见。

```
//Need to have a reference to ConfigurationContext
ConfigurationContext configContext = getConfigurationContext();
File serviceArchiveFile = new File("Location of the file");
//Now let's create AxisServiceGroup which contains the service we want to have
AxisServiceGroup serviceGroup = DeploymentEngine.loadServiceGroup(
    serviceArchiveFile,
    configContext);
```

一旦你创建了服务，下一步就是将服务添加到系统中。你可以按如下方式操作：

```
//Getting a pointer to AxisConfiguration
AxisConfiguration axiConfiguration = configContext.
getAxisConfiguration();
//Adding the created service
axiConfiguration.addServiceGroup(serviceGroup);
```

现在，如果你尝试使用以下 URL 访问 add 方法，你应该得到与之前相同的结果：

```
http://localhost:6060/axis2/services/MyService/add?a=10&b=15
<ns:addResponse>
    <return>25</return>
</ns:addResponse>
```

## 总结

在本章中，我们讨论了 Axis2 部署的工作方式、可用部署描述符的类型及其结构。此外，我们还讨论了 Axis2 中可用的不同类型的配置文件。在本章末尾，我们学习了最重要的内容，即 Axis2 中可用的部署选项。我们也通过示例测试了这些选项。下一步将是创建更多服务并看看会发生什么。

在下一章中，我们将讨论更多关于 Axis2 信息模型的内容。在那里，我们将讨论静态和动态数据层次结构、层次结构中的不同实体、它们是如何创建的，以及哪些描述符负责不同的实体。

