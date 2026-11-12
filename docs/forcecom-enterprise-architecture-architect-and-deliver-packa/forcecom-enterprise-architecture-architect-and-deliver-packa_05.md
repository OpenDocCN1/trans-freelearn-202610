# 企业应用架构模式

到目前为止，我们仅讨论了来自执行上下文（Apex Controller、Scheduler、Batch Apex等）的Apex代码与放置在`Service`类中的可复用应用程序业务逻辑代码之间的关注点分离。然而，还有更细的粒度和模式，有助于进一步聚焦和封装应用程序逻辑，这些被称为企业应用架构模式。

本书接下来三章中使用的模式的一般定义本身并不新鲜，但对该平台来说是一种新的实现。这些模式在其他平台上一直非常流行，并且持续流行。这些模式的原始作者是Martin Fowler，他在其著作*企业应用架构模式*（http://www.martinfowler.com/books/eaa.html）中描述了其他模式。

## 服务层

以下是Martin Fowler对服务层的定义（http://martinfowler.com/eeaCatalog/serviceLayer.html）：

> “使用一层服务定义应用程序的边界，该层建立一组可用操作，并在每个操作中协调应用程序的响应。”

在本章中，您已经初步了解了该模式的实际应用。可以看到，对于业务应用程序逻辑，它构成了一个初始入口点，封装了最终用户执行的操作和任务。

其方法和行为的设计使其对调用方不可知，从而可以轻松适应多个执行上下文，如前面的示例所示。它还在您的应用程序API策略中发挥作用。

## 领域模型层

以下是Martin Fowler对领域层的定义（http://martinfowler.com/eeaCatalog/domainModel.html）：

> “一个包含行为和数据的领域对象模型。在最糟糕的情况下，业务逻辑可能非常复杂。规则和逻辑描述了许多不同的行为案例和侧面，正是这种复杂性是对象设计所要处理的。”

如果将服务层视为乐队指挥，那么构成领域层的Apex类就是它指挥的乐器，每个乐器在奏响乐章中都有自己的角色和职责！

## 数据映射器（选择器）层

以下是Martin Fowler对数据映射器层的定义（http://martinfowler.com/eeaCatalog/dataMapper.html）：

> “一个映射器层，用于在对象和数据库之间移动数据，同时保持它们彼此独立，也独立于映射器本身。”

在Force.com上，充分利用数据库查询来获取数据与其他平台一样重要。对于复杂的对象模型，让SOQL承担部分繁重工作可以产生相当复杂的查询。封装数据库数据如何映射到内存中是数据映射器层的职责。

在Martin Fowler的世界中，这通常涉及将返回的数据库结果集记录映射到Java类（称为**简单老式Java对象 (POJO)** 或 **数据转换对象 (DTO)**）。然而，Apex为我们的每个自定义对象以SObject的形式提供了这些数据结构。因此，此模式的角色略有不同，因为SOQL查询本身明确选择了代表数据的SObject。因此，此模式已被重命名，以更准确地反映其关键职责。

## 介绍FinancialForce.com Apex Commons库

存在一个用于支持这些模式的Apex类库，本书也将使用它。Apex Commons库源于FinancialForce.com发起的一个开源项目，并于Dreamforce 2012活动期间向社区发布。从那以后，这些模式已被平台上的其他开发人员采用，并通过Fieldsets和Security等附加功能进行了扩展。在接下来的三章中，我们将使用此库来更详细地探讨其支持的企业模式。GitHub仓库位于 https://github.com/financialforcedev/fflib-apex-common.

## 单元测试与系统测试

说到测试Apex代码，我们知道该怎么做：编写良好的测试来覆盖您的代码，断言其行为，并获得至少75%的代码覆盖率。Force.com不会允许您上传打包代码，除非您获得此比例或更高的覆盖率。您还必须覆盖Apex触发器代码，即使只有一行，正如您很快将在本书中看到的Apex触发器实现情况一样。

然而，Force.com目前缺少一个模拟框架，以允许对前面各节中提到的各层进行更集中和隔离的测试，而无需设置执行要测试的代码所需的所有记录。这使得您的Apex测试开始更像系统级测试，每次都必须执行完整的功能堆栈。

虽然测试驱动开发（TDD）等惯例是可能的，它们确实能帮助您思考在平台上开发真正的单元测试。但由于数据设置复杂性不断增加，这样做开发起来也非常耗时，然后还要等待这些测试运行。此类测试最终感觉更像系统级或集成级测试，因为它们最终会测试应用程序中的整个代码堆栈。

好消息是，本书中描述的分层确实为您提供了将较小的测试隔离到特定方面（例如验证或插入逻辑）的机会。在接下来的章节中，我们将逐层深入，并以一整章的篇幅结束，专注于使用名为Apex Mocks的开源模拟框架来实施真正的单元测试方法。该框架由FinancialForce.com与社区共享，并大量基于Java Mockito框架。https://github.com/financialforcedev/fflib-apex-mocks.





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
