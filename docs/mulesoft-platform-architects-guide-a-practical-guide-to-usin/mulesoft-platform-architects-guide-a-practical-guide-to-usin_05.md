# 借助加速器加速

在本章中，我们将探讨直接从 `MuleSoft` 获取并可在 `Anypoint Exchange` 中找到的 `artifacts`（工件）、`pre-built frameworks`（预构建框架）和 `APIs`。这些组件统称为 `accelerators`（加速器）。这些 `accelerators` 可以在架构和开发方面加快项目或组织的进度。它们还突出并展示了许多标准和最佳实践，可供学术研究以用于教学目的。对于刚开始处理其 API 环境的组织，这些标准和最佳实践可能足以增强或丰富现有的（或缺失的）公司标准和最佳实践。

## 涵盖主题

- 了解 `MuleSoft` 开发并包含在 Exchange 中的 `accelerators`
- 理解使用 `accelerator` 如何比从零开始更快地让您的组织启动并运行
- 了解构成这些 `accelerators` 的构建块
- 批判性地思考 `accelerators` 的一些局限性
- 简要回顾 `Catalyst` 的 `GitHub` 存储库中发现的其他加速工件




# 拆解加速器构建模块

## 简介

加速器是文档和技术资源的集合，主要由 API 规范、实现模板、连接器和特定行业加速器组成。这些 API 策略加快了各种用例的实施，解决了重大的业务或行业问题。加速器的设计初衷是像模块化构建组件一样运作，允许客户修改端点、映射和模式以满足其需求。它们代表了 MuleSoft 建议客户遵循的最佳实践，以完成特定用例。每次使用加速器资产都能为客户节省无数小时的发现、设计、开发和测试时间。

此外，还可以创建自定义加速器和可复用框架，例如错误处理、日志记录、审计、自定义 API 策略和连接器。这些是使任何企业能够提高效率并加快 API 构建的基本组件。要使用任何加速器，您需要登录 Anypoint Platform。Anypoint Exchange 提供了大量预构建的加速器，涵盖金融服务、制造、医疗保健、零售和生命科学等行业。此外，还提供适用于各种系统的加速器，例如 SAP、OMS 和 Data Cloud。要查找 MuleSoft 提供的一些加速器，请点击以下链接：`https://www.mulesoft.com/exchange/org.mule.examples/mulesoft-accelerators-introduction/`。

本章的目标是让架构师了解这些加速器如何在 API 生命周期的各个阶段节省时间。您将了解每个加速器中的构建模块如何被使用和定制，以及包含了哪些不同的资产、模式、映射和端点。沿用前面描述的 J&J Music Store 用例，您将演练设置和定制零售加速器的部分内容。

在本章中，我们将引用 Anypoint Exchange 中的加速器。这些加速器的列表在以下 Anypoint Exchange 自定义资产中进行了描述和链接：`https://anypoint.mulesoft.com/exchange/org.mule.examples/mulesoft-accelerators-introduction/minor/1.0/`。

特定的零售和消费品加速器可以在此处找到：`https://anypoint.mulesoft.com/exchange/org.mule.examples/mulesoft-accelerator-for-retail/`。

除了 Exchange 中的这些资源外，我们还将简要查看 GitHub 上 MuleSoft Catalyst 下的一些资产。目前，该组织的 GitHub 账户拥有 157 个代码库，可在此处找到：`https://github.com/mulesoft-catalyst`。

要使用和查看本章所示的示例，您需要安装 Anypoint Studio 或安装了 Anypoint Code Builder 扩展包的 Visual Studio Code。

## 核心构建模块

MuleSoft 的加速器作为基础元素，旨在加快和简化 API 生命周期的各个阶段。这些加速器可通过 Anypoint Exchange 获取，是预构建 API、连接器、模板和最佳实践的精选集合。每个加速器都旨在满足特定行业需求，实现 API 解决方案的快速开发、集成和部署。

在本节中，我们将研究这些加速器的核心组件，即*构建模块*，并解释它们如何提高效率和定制化。主要资产包括以下内容：

*   预构建 API
*   连接器
*   模板
*   最佳实践
*   数据映射
*   端点

让我们更详细地检查这些资产，重点关注它们的描述、优势和使用示例。

### 预构建 API

**预构建 API** 是功能完整的 API，旨在执行与加速器关注领域相关的特定任务。这些 API 可以管理数据检索、处理、转换以及与各种系统的集成。

#### 优势

预构建 API 的优势如下：

*   **节省时间**：无需从头构建 API。
*   **标准化**：确保一致且符合最佳实践的实施。
*   **可扩展性**：设计用于处理企业级数据量和事务。

#### 使用示例

在零售加速器中，预构建 API 可以通过与商店的库存管理系统交互来管理产品库存。

### 连接器

**连接器** 促进 MuleSoft 应用程序与外部系统（如数据库、SaaS 应用程序和第三方服务）之间的无缝通信。

#### 优势

连接器的优势如下：

*   **易于集成**：简化了连接不同系统的过程。
*   **灵活性**：支持多种协议和数据格式。
*   **可复用性**：连接器可跨不同项目和应用程序使用。

#### 使用示例

零售加速器中的连接器可以将零售音乐店的电商平台与其 CRM 系统链接，实现客户数据同步。

### 模板

**模板** 提供预配置的、可复用的集成模式。它们充当常见集成场景的蓝图，降低了设置集成的复杂性和所需时间。

#### 优势

模板的优势如下：

*   **速度**：加速集成的初始设置。
*   **一致性**：确保集成模式的统一实施。
*   **可定制性**：可根据特定业务需求进行调整。

#### 使用示例

零售加速器包含一个模板，用于在线商店和履行系统之间同步客户订单。

### 最佳实践

**最佳实践** 是嵌入在加速器中的指南和建议。它们指导用户走向最优的 API 设计、安全性和治理。

#### 优势

优势如下：

*   **质量保证**：促进 API 开发和管理的高标准。
*   **安全性**：确保 API 设计具备强有力的安全措施。
*   **效率**：鼓励高效且可维护的代码实践。

#### 使用示例

零售加速器可能包含处理敏感客户数据的最佳实践，确保符合数据保护法规。

### 数据映射

**数据映射** 定义数据应如何在不同系统和格式之间转换和传输。它们确保数据在集成系统之间平稳、正确地流动。

#### 优势

数据映射的优势如下：

*   **准确性**：最大限度地减少数据翻译和转换中的错误。
*   **互操作性**：确保不同系统能够有效通信。
*   **可定制性**：可调整以适应特定的数据结构。

#### 使用示例

零售加速器的数据映射可以将电商平台的销售数据转换为适合会计系统的格式。

### 端点

**端点** 是 API 可访问的特定 URL 或地址。它们定义了 API 交互的入口点，指定 API 如何以及在何处被调用。

#### 优势

端点的优势如下：

*   **可访问性**：为 API 消费者提供清晰、结构化的访问点。
*   **控制力**：允许对 API 访问和安全性进行细粒度管理。
*   **可扩展性**：支持根据需要扩展 API 访问。





## 使用示例

在零售加速器中，可能会为处理客户反馈的 API 建立一个端点。这将实现评论的轻松提交和检索。

# 自定义 MuleSoft 加速器

MuleSoft 加速器的自定义涉及修改加速器预构建的 API、集成模板和数据映射，以更好地满足您独特的业务需求。MuleSoft 加速器可以通过以下方式进行自定义：

*   **API 自定义**：MuleSoft 在设计 API 时充分考虑了最佳实践和行业标准。然而，每家公司的需求可能各不相同。通过在 API 设计器中修改 API 规范，您可以定制这些 API 以满足您的特定需求。

*   **数据映射自定义**：MuleSoft 加速器提供预制的数据映射，用于在各种系统之间转换和映射数据。然而，由于每家公司都有独特的数据结构，您可能需要修改这些映射以满足您的特定需求。

*   **集成资产自定义**：加速器提供的集成模板可进一步自定义，以满足您独特的用例需求。可以修改这些模板以添加新步骤、移除不必要的步骤，或更改系统间的数据传输方式。

*   **流程流自定义**：流程流指定了各个系统和流程之间的交互。您可能需要修改这些流程以适应您的需求。

请记住，尽管自定义很有效，但应谨慎进行。确保更改安全、与系统其余部分兼容，且不会无意中干扰其他功能。如果不确定，请咨询 MuleSoft 专家。

## 自定义零售加速器——J&J 音乐店提速

为了说明这些构建模块如何协同工作，请考虑一家零售音乐店实施零售加速器的用例。该店旨在通过集成其电商平台、库存系统和 CRM 来简化运营。J&J 正寻求实现一些用例，如实时库存同步、客户档案同步、产品同步和销售订单同步。J&J 使用 `Salesforce` 作为 CRM 系统，`SAP` 作为 ERP 系统，并使用另一个电商平台用于 B2C 和 B2B。MuleSoft 提供的零售加速器涵盖了 J&J Music 所需实现的大部分内容。

以下是使用和自定义 MuleSoft 提供的零售加速器的步骤：

1.  **设置**：该店利用预构建的 API 来管理产品库存、客户数据和销售交易。

2.  **连接系统**：使用连接器，该店将其电商平台连接到库存和 CRM 系统，确保数据在它们之间无缝流动。

3.  **应用模板**：该店利用模板在其在线商店和后端系统之间设置数据同步。

4.  **遵循最佳实践**：实施遵循安全和数据管理的最佳实践，确保运行稳健且合规。

5.  **映射数据**：配置数据映射以在不同系统之间准确转换和传输数据。

6.  **定义端点**：为 API 建立端点，实现对 API 功能的清晰且安全的访问。

通过利用这些构建模块，J&J 音乐店不仅将加速其集成流程，还将确保拥有一个强大、可扩展且高效的系统，满足其业务需求。

J&J Music 决定构建自定义框架、模板和连接器，以提高整体效率和生产力。为什么要构建自定义框架和加速器？有时组织无法在 `Anypoint Exchange` 中找到所需的加速器、连接器或框架。同时，他们知道这些组件将来会被多个 API 所需求。这将提高整个组织的整体可复用性并减少开发工作量。

J&J Music 构建了以下组件并将其发布到 `Anypoint Exchange`，以便组织中的任何人都可以利用。以下组件由 J&J Music 构建：

*   **错误处理框架**：构建了一个错误处理框架并将其作为 Mule 插件发布到 `Anypoint Exchange`。开发人员只需在 `POM.xml` 中添加 Maven 依赖项，即可轻松在其应用程序中使用此框架。这是一个专门为 J&J Music 定制的通用错误处理框架，它确保了整个组织错误处理的一致性。

*   **日志记录框架**：J&J Music 决定使用开源的 `JSON Logger` 并将其发布到 `Anypoint Exchange`。他们根据自己的需求定制了 `JSON Logger` 并增加了额外功能。`JSON Logger` 将确保整个组织的日志记录保持一致且采用 JSON 格式。

*   J&J Music 决定将应用程序属性存储在 `Azure vault` 中，并构建了一个 `Azure vault properties loader`。此加载器的功能类似于开箱即用的配置属性读取器，直接从 `Azure vault` 提取属性。

*   J&J Music 还决定为 API 和批处理应用程序构建模板。这些模板将作为开发人员构建 API 时的起点。

*   **API 片段** 是 API 规范的片段，提供了一种在规范中重用 API 部分或片段的方法，从而能够更快地创建 API。API 片段和可复用 API 对于弥合 IT 交付差距至关重要。

借助通用框架和资产，J&J Music 的整体生产力和效率得到了提高。

正如我们在上一节所见，J&J 正在使用零售加速器来加快开发并总体提高产出。现在让我们讨论一下 MuleSoft 加速器提供的一些结构要素和优势。

# MuleSoft 加速器的基本构建模块

MuleSoft 加速器提供了多个构建模块，有助于加快构建应用程序网络的过程。以下是构成这些加速器的一些基本构建模块：

*   **预构建 API**：这些是预先开发和设计的 API，涵盖了一系列标准业务对象和功能，包括订单、供应商、患者和产品。这些 API 可以快速轻松地导入到任何 MuleSoft 应用程序中以供即时使用。它们还符合 `RESTful API Modeling Language` (`RAML`)。

*   **集成资产**：集成资产包含预构建的连接器、模板和示例，可促进不同系统的快速轻松集成。它们还有助于减少开发时间。

*   **API 主导的连接性**：此组件使资产能够被轻松发现和复用，从而提高开发速度并降低出错可能性。

*   **数据映射**：预定义的数据映射有助于将数据结构从一种格式转换为另一种格式，使系统间的信息交换更轻松、更无缝。

*   **流程流**：这些是预制的设计，展示了各种系统和 API 通常如何连接以实现业务流程自动化。

*   **最佳实践**：为了有效指导用户，加速器将 MuleSoft 和行业最佳实践集成到其组件中。

*   **安全属性**：这些属性对凭据和其他敏感数据进行加密并安全存储。这使您能够保护 API 和集成的安全。

结合在一起时，这些要素为利用 MuleSoft 高效快速地启动集成项目提供了坚实的基础。此外，这些组件可以修改以满足公司的独特需求，从而增强 MuleSoft 加速器的灵活性和实用性。




# MuleSoft 加速器：优势与局限性

## 优势

以下是 MuleSoft 加速器提供的优势：

*   **缩短开发时间**：组织可以通过使用预构建的 API 和集成模板，减少从头创建集成所需的时间。因此，项目时间表得以加快，实现了更快的实施和更短的上市时间。
*   **降低复杂性**：MuleSoft 加速器包含预定义的数据映射，用于在各种系统之间转换和映射数据。这简化了数据转换过程，并减少了错误和不一致性。
*   **提高效率**：公司通过节省开发新集成和故障排除的时间，能更有效地使用集成。这可以通过利用预先存在的模板和数据映射来实现。
*   **成本节约**：MuleSoft 加速器通过减少开发时间和工作量，可带来中长期的成本节约。
*   **合规性与最佳实践**：MuleSoft 加速器在设计时就考虑了合规性和最佳实践，这意味着它们遵循行业标准。
*   **加速数字化转型**：加速器使企业能够快速集成不同的系统并简化手动流程，从而加快数字化转型进程。
*   **可扩展性**：MuleSoft 加速器为企业提供可扩展的集成解决方案，能适应其需求。它们支持根据需要轻松扩展或修改。

虽然 MuleSoft 加速器提供了许多优势，但企业也必须考虑潜在的缺点。这些可能包括定制化需求，或管理和维护 MuleSoft 平台所需的资源。

## 局限性

以下是在使用 MuleSoft 加速器时可能遇到的局限性，这些完全取决于组织的需求：

*   **技术专长**：使用 MuleSoft 加速器需要技术专长。MuleSoft Anypoint 平台对新用户可能呈现陡峭的学习曲线。由于平台对广泛集成概念的要求，可能存在准入门槛，导致集成进度放缓。
*   **定制化**：MuleSoft 提供了许多加速器和预构建 API。然而，这些并不总能涵盖独特的业务需求，从而导致定制化需求。此过程需要专业技能，并可能增加项目时间和成本。
*   **学习曲线**：由于 MuleSoft Anypoint 平台陡峭的学习曲线，充分利用 MuleSoft 加速器通常需要对其有深入的理解。
*   **维护**：加速器和预构建模板加快了初始开发和部署，但可能增加对 MuleSoft 进行未来维护和更新的依赖。这可能导致长期成本增加。
*   **可扩展性问题**：使用 MuleSoft 加速器可能无法快速扩展运营。这取决于贵公司订阅的 MuleSoft 套餐，以及该套餐是否允许大量 API 调用。因此，它可能无法满足突发、快速增长的需求。
*   **成本**：教育员工使用平台、修改预构建集成以及保持集成更新可能产生额外费用。
*   **兼容性问题**：加速器设计用于与广泛的系统配合工作。然而，在某些情况下，它可能无法与较新的复杂系统或较旧的遗留系统集成。
*   MuleSoft 加速器的**“一刀切”方法**提供预构建的数据连接器、集成模板和 API。然而，这些可能过于通用，无法有效解决特定的、专业的或复杂的业务需求。定制这些功能以满足需求可能需要大量时间和工作。

组织在决定 MuleSoft 加速器是否为其集成需求的最佳投资时，必须了解其局限性。

# MuleSoft Catalyst GitHub 代码库

MuleSoft Catalyst 结合了资源、服务和以结果为导向的方法论，以优化投资回报并推动公司转型。它是一系列资源、最佳实践和建议的集合，企业可利用这些加速数字化转型。反之，GitHub 是一个流行的基于 Web 的版本控制托管服务，大多数开源项目在此发布其公开可用的代码。

MuleSoft Catalyst GitHub 是 MuleSoft 的官方 GitHub 账号，公司在其中分享连接器、预构建 API、集成模板和其他组件，组织可利用这些加速开发。MuleSoft 通过在 GitHub 等平台上公开这些资源，简化了开发人员查找、使用和贡献其工具的流程。这带来了更快、更高效的开发周期。它提供了大量可用于快速开发应用程序的资源。

## 可用组件

MuleSoft GitHub 代码库中提供以下组件，组织可利用这些组件提高生产力并加速开发：

*   预构建的 API 模板和 API
*   连接器和属性加载器
*   示例应用程序
*   DataWeave 示例
*   架构指南和最佳实践
*   实施指南
*   通用框架（如错误插件）
*   API 策略
*   RAML 片段

重要的是要理解，访问这些资源仍然需要对 MuleSoft 及其 Anypoint 平台有专家级的理解。

## 代码库中的关键组件

让我们讨论一下 MuleSoft Catalyst GitHub 代码库中发现的有用组件。

| 组件名称 | 组件类型 | 描述 | 链接 |
| --- | --- | --- | --- |
| 指标工具包 | MuleSoft 应用程序 | 指标工具包（前身为指标加速器/框架）是一个 Mule 应用程序，旨在收集、编译平台指标并将其加载到各种可视化工具中。它提供预配置的可视化选项和集成，例如实用的仪表板和图表。 | [`github.com/mulesoft-catalyst/metrics-toolkit`](https://github.com/mulesoft-catalyst/metrics-toolkit) |
| 熔断器策略 – Mule 4 | API 策略 | 它是一种轻量级 API 策略，用于以更简单的方式实现熔断器模式。 | [`github.com/mulesoft-catalyst/circuit-breaker-policy-mule-4`](https://github.com/mulesoft-catalyst/circuit-breaker-policy-mule-4) |
| 错误处理插件 | MuleSoft 插件 | 它是一个错误处理插件，用于在 MuleSoft 应用程序中实现错误处理。 | [`github.com/mulesoft-catalyst/error-handler-plugin`](https://github.com/mulesoft-catalyst/error-handler-plugin) |
| 金丝雀策略 – Mule 4 | API 策略 | 它是一种 API 策略，用于以更简单的方式实现金丝雀部署模式。 | [`github.com/mulesoft-catalyst/canary-policy-mule-4`](https://github.com/mulesoft-catalyst/canary-policy-mule-4) |
| Vault 属性加载器 | Mule 连接器 | 该连接器用于从 Vault 加载属性。可定制以连接任何 Vault，如 HashiCorp、Azure Vault 和 AWS Vault。 | [`github.com/mulesoft-catalyst/properties-loader-vault-mule4`](https://github.com/mulesoft-catalyst/properties-loader-vault-mule4) |
| Mule SonarQube 插件 | MuleSoft 插件 | 借助 Mule SonarQube 插件，可以使用 SonarQube 对 Mule 项目进行代码检查和项目指标收集。 | [`github.com/mulesoft-catalyst/mule-sonarqube-plugin`](https://github.com/mulesoft-catalyst/mule-sonarqube-plugin) |



## 表 5.1：MuleSoft Catalyst GitHub 加速器

| 名称 | 类型 | 描述 | 链接 |
|------|------|-------------|------|
| RTF CloudHub 核心计算器 API | MuleSoft 应用程序 | 使用此 API，您可以确定您的组织和业务组正在使用多少 Runtime Fabric 核心。这将帮助您监控核心使用情况，并确保您不会超出许可证限制。 | [`github.com/mulesoft-catalyst/rtf-cloudhub-cores-calculator-api/`](https://github.com/mulesoft-catalyst/rtf-cloudhub-cores-calculator-api/) |
| JSON Logger | MuleSoft 连接器 | MuleSoft 扩展 JSON Logger 可在此 GitHub 存储库中获取。借助该扩展的众多功能，您可以最大化每条日志的价值并改善您的日志记录体验。 | [`github.com/mulesoft-consulting/json-logger`](https://github.com/mulesoft-consulting/json-logger) |

MuleSoft Catalyst 的 GitHub 存储库中提供的组件比上表中列出的更多。您可以在此处找到它们：[`github.com/mulesoft-catalyst`](https://github.com/mulesoft-catalyst)。

## 摘要

本章介绍了组织利用 MuleSoft 加速器提高工作效率和加快开发速度的方法。还讨论了使用这些加速器的潜在缺点。此外，我们还研究了 MuleSoft Catalyst GitHub 提供的组件以及创建加速器所需的构建模块。

在下一章中，我们将讨论如何将功能需求与期望的业务成果相匹配。

## 延伸阅读

- **MuleSoft 加速器**：[`www.mulesoft.com/exchange/org.mule.examples/mulesoft-accelerators-introduction`](https://www.mulesoft.com/exchange/org.mule.examples/mulesoft-accelerators-introduction)
- **MuleSoft Catalyst GitHub 存储库**：[`github.com/mulesoft-catalyst`](https://github.com/mulesoft-catalyst)
