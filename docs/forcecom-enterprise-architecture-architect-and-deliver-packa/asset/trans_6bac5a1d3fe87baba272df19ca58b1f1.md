

## 打包代码

本章提供的源代码包含本章前面使用的 UML 图中所示的 Apex 类框架。在接下来的章节中，我们将完善其中的方法和逻辑。目前，请将它们部署到您的打包组织中并添加到您的包中。请注意，一旦添加 Apex 类，它们随后引用的任何 Apex 类都会自动拉入包中。以下是本章添加的 Apex 类列表及其适用的应用架构层：

| Apex 类 | 层 |
| :--- | :--- |
| SeasonController.cls | Visualforce 控制器 |
| SeasonControllerTest.cls | Apex 测试 |
| ContestantController.cls | Visualforce 控制器 |
| ContestantControllerTest.cls | Apex 测试 |
| RaceController.cls | Visualforce 控制器 |
| RaceControllerTest.cls | Apex 测试 |
| SeasonNewsletterScheduler.cls | Apex 调度器 |
| SeasonNewsletterSchedulerTest.cls | Apex 测试 |
| RaceService.cls | 赛事服务 |
| RaceServiceTest.cls | Apex 测试 |
| SeasonService.cls | 赛季服务 |
| SeasonServiceTest.cls | Apex 测试 |
| ContestantService.cls | 参赛者服务 |
| ContestantServiceTest.cls | Apex 测试 |

请注意，有一个类 SeasonNewsletterScheduler 需要使用 global 访问修饰符。这确保了该类在未来版本中无法被删除或重命名。这可能感觉受限，但在这种情况下是支持 Apex Scheduler 功能的关键。当用户升级到您的包的新版本时，他们定义的计划继续引用您的包中的代码这一点很重要。

上传包的新版本并将其安装到您的测试组织中。您应该会看到类似以下的确认安装摘要页面：

| 操作 | 组件名称 | 父对象 | 组件类型 | 安装说明 |
| :--- | :--- | :--- | :--- | :--- |
| 创建 | SeasonNewsletterScheduler | | Apex 类 | 这是一个全新的组件。 |
| 创建 | SeasonController | | Apex 类 | 这是一个全新的组件。 |
| 创建 | SeasonService | | Apex 类 | 这是一个全新的组件。 |
| 创建 | RaceService | | Apex 类 | 这是一个全新的组件。 |
| 创建 | RaceServiceTest | | Apex 类 | 这是一个全新的组件。 |
| 创建 | SeasonNewsletterSchedulerTest | | Apex 类 | 这是一个全新的组件。 |
| 创建 | RaceController | | Apex 类 | 这是一个全新的组件。 |
| 创建 | ContestantServiceTest | | Apex 类 | 这是一个全新的组件。 |
| 创建 | SeasonControllerTest | | Apex 类 | 这是一个全新的组件。 |
| 创建 | ContestantControllerTest | | Apex 类 | 这是一个全新的组件。 |
| 创建 | ContestantController | | Apex 类 | 这是一个全新的组件。 |
| 创建 | RaceControllerTest | | Apex 类 | 这是一个全新的组件。 |
| 创建 | SeasonServiceTest | | Apex 类 | 这是一个全新的组件。 |
| 创建 | ContestantService | | Apex 类 | 这是一个全新的组件。 |

## 总结

在本章中，我们深入探讨了平台如何执行 Apex 代码以及执行的不同上下文。我们还花时间了解状态和安全性等关键概念是如何管理的，并重点介绍了一些 Apex 限制及其各自的范围。

这使我们能够观察到一些常见需求，并将其提炼为关注点分离（Separation of Concerns），我们可以开始将其作为准则应用于业务应用程序代码的各层中，从而使 Apex 逻辑更加可复用，并能随着您的应用程序功能（甚至平台本身）的发展而从不同上下文中访问。

随着我们在实践关注点分离方面不断深入，Domain、Service 和 Selector 层的模式将成为我们应用程序业务逻辑内进一步的分离层。在接下来的三章中，我们将继续定义命名和编码准则，以便于现有开发人员和刚开始使用代码库的开发人员更容易维护良好的关注点分离。

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_163_0.png)

如果您有现有的代码库，并且不知道从哪里开始将其中的代码重构到本章描述的各层中，我的建议是：最初专注于形成您的 Service 层契约，并尽可能将真正的业务逻辑代码重构到其中。使您的执行上下文类（例如控制器和 Batch Apex 类，但最初不包括触发器）尽可能精简，让它们专注于自身的职责，如错误处理。这将至少使您的一些真正的业务应用程序逻辑包含在 Service 层之下，即使它可能不是您希望从零开始编写时所要的精确形式。之后，您可以专注于将触发器逻辑重构到 Domain 层，再将查询逻辑重构到独立的 Selector 类中，或许可以随着您对应用程序增强区域的修订而逐步进行。

## 应用程序服务层

如果将您的应用程序视为一个生物体，Service 层将是其跳动的心脏。无论环境及其交互的事物随时间如何变化，它都必须保持强大并能够适应。在本章中，我们将以三种编码模式——Service、Domain 和 Selector——开始我们的旅程，这些模式在第 4 章“Apex 执行与关注点分离”中已介绍。

在本章中，我们将回顾马丁·福勒阐述的该模式，然后回顾它在 Force.com 平台上的 Apex 中如何应用，描述源自我们在上一章中定义的关注点分离的设计准则。

本层的一个关注点是与数据库交互；后续章节将更详细地介绍查询。本章将侧重于更新数据库，并引入一种新模式——工作单元（Unit Of Work），它有助于使您的代码更精简并支持批量处理。

在本章末尾，我们将扩展 FormulaForce 应用程序，以提供计算车手锦标赛积分和发送通讯的方法。在 Service 层中开发的代码将从自定义按钮和 Apex Scheduler 调用。后续章节还将介绍 Service 层的更多用法。

本章将介绍 Service 层的以下方面：

- 介绍 Service 层模式
- 实现设计准则
- 使用工作单元处理 DML
- 服务调用服务
- 契约驱动开发
- 测试服务
- 调用服务

## 介绍 Service 层模式

以下是马丁·福勒对 Service 层的定义（http://martinfowler.com/eaaCatalog/serviceLayer.html）：

> "使用服务层定义应用程序的边界，建立一组可用操作，并协调应用程序在每个操作中的响应。"

马丁定义中使用“边界”一词很有趣，因为它从字面上代表了 Service 层中的应用程序业务逻辑关注点与执行上下文或调用者（无论是 Visualforce 或 Lightning Component 控制器类还是 Batch Apex 类）之间的分离点或边界，如前一章所示的 UML 图所插图。

以下插图展示了 Apex Service 层设计用于支持的一些调用者类型。通过遵循下一个图中给出的设计准则，您可以确保 Service 层代码未来可以从这些功能中的任何一个以及其他功能中调用：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_165_0.png)

本书将在若干这些领域中说明 Service 层的使用。

马丁定义中对边界的引用也是封装应用程序真正业务逻辑的一个重要方面。您必须谨慎对待这种封装，因为没有它，Service 层无法协调应用程序的行为。本书中的模式有助于促进关于封装的更好思考。然而，不存在像 Java 等其他语言可能使用 `package` 和 `protected` 关键字那样表达 Apex 代码之间依赖关系的方式。因此，我们必须确保对它们有坚实的认识和遵循意愿。

> 代码审查是任何开发过程不可或缺的一部分。一个容易表明您的应用程序业务逻辑可能正在泄露到 Service 层之外的迹象是，调用类（例如 Visualforce 或 Lightning Component 控制器，或 Batch Apex 类）中的代码量在不断增加。请记住，这些类也有自己的关注点，例如处理和报告错误以及展示 Service 层或自定义对象所需的信息。不过，一般来说，如果代码看起来在做的事情超出了这些范围，这可能意味着有些代码本应被提取到 Service 方法中。困难的部分往往是基于这一观察未能通过代码审查，特别是如果作者声称目前没有复用案例。虽然这可能是事实，但最初忽略它并让它保留会增加技术债务，限制复用（下一位开发人员会有时间重构吗？），并增加整个应用程序中行为不一致的风险。

最后，请记住，与 Service 层交换的信息不一定针对特定用例或调用者要求。将所选客户端用户界面与 Service 层界面之间进行转换的责任在于调用代码，例如 Visualforce 或 Lightning Component 控制器，甚至是订阅组织开发人员编写的代码。

在第 10 章，*提供集成与扩展性*中，我们还将探讨将 Service 层作为 API 对外公开。出于这些原因，Service 层必须对任何特定调用者保持中立或无偏见。

