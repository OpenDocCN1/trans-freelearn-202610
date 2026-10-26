

## EDB Postgres Advanced Server

**EnterpriseDB Postgres Advanced Server** 是一个集成的、企业级版本的开源 **PostgreSQL** 数据库，具备先进的企业安全、性能诊断、Oracle 兼容性以及其他生产力特性：

![](img/50e1dac0750b69f2b8e8efff000eb13b_163_0.png)

图 7.5 – EDB Postgres Advanced Server

EDB Postgres Advanced Server 的用例包括以下方面：

- 适用于新应用的 DevOps、无模式、多编程语言支持的快速开发
- 需要多模式灵活性和与主流数据源集成的应用现代化
- 遗留迁移需要 Oracle 兼容性
- 支持向云迁移的灵活部署选项

## MongoDB Enterprise Advanced

**MongoDB Enterprise Advanced** 是一个高度可扩展、面向文档的 NoSQL 数据库，支持动态模式，使其更容易在特定类型的应用程序中集成数据。Cloud Pak for Data 中的 MongoDB 服务是一个具有自动扩展功能的高可用数据库。

MongoDB Enterprise Advanced 还为您提供全面的运维工具、高级分析和数据可视化、平台集成以及认证：

![](img/50e1dac0750b69f2b8e8efff000eb13b_164_0.png)

图 7.6 – MongoDB Enterprise Advanced

MongoDB 的用例包括以下方面：

- 遗留应用现代化，需要不断演进的模式
- 将企业数据作为服务提供，需要组合来自不同来源的数据
- 实施云数据战略，需要可扩展性和可移植性

在下一节中，我们将讨论如何组织扩展服务。

## 组织扩展服务

AI 阶梯的“收集（Collect）”层级是将数据放入适当的持久化存储并实现轻松访问，而“组织（Organize）”扩展服务则致力于构建可信的分析基础。这涵盖数据准备、数据转换、数据治理、数据隐私、主数据管理以及企业范围内数据的生命周期管理。虽然 Cloud Pak for Data 的基础产品包含一组丰富的数据准备、数据治理和数据隐私功能，但 Organize 层级的其余部分可通过以下四个扩展服务（也称为扩展包）获得：

### DataStage

数据集成是任何企业的关键方面，随着企业数据资产的规模不断增加和分布式特性（通常分布在多个云和数据中心），它变得更加重要。IBM DataStage 是企业领先的提取、转换、加载（ETL）产品，拥有数千家客户，以其可扩展性和运营效率而闻名。您可以使用 Cloud Pak for Data 上的 DataStage 服务来设计和运行移动及转换数据的数据流，这使您能够快速、准确地组合和运营数据流。通过直观的图形设计界面，您可以连接到各种数据源，集成和转换数据，并以批处理或实时方式将其交付到目标系统。

这两种服务都为您的数据流提供了数百个开箱即用的内置业务操作。支撑 DataStage 的高性能并行运行时引擎使企业能够扩展以满足其集成需求，即使数据量和数据复杂性不断增加。

Cloud Pak for Data 上的 DataStage 有以下两种版本：

- DataStage Enterprise
- DataStage Enterprise Plus

DataStage Enterprise 帮助设计数据集成作业，并允许数据工程师为多个源和目标转换及移动数据。您可以创建可重用的作业模板，或在直观的图形界面中设计数据集成流。数据工程师还可以使用各种转换器和阶段来设计从简单到复杂的作业，而高性能运行时引擎则支持可扩展性并高效集成大量数据。

DataStage Enterprise Plus 包含 DataStage Enterprise 的所有功能，并额外提供了用于数据质量的有用功能。这些功能包括：

- 通过识别潜在异常和元数据差异来清洗数据
- 通过数据匹配和两个数据集之间数据实体的概率匹配来识别重复项

![](img/50e1dac0750b69f2b8e8efff000eb13b_166_0.png)

图 7.7 – DataStage 扩展包

### Information Server

Cloud Pak for Data 上的 IBM **Information Server** 扩展包为 IT 和**业务线（LOB）**用户提供了一套全面的功能，用于组织所有企业数据。它是一套服务，包括处理数据集成、数据质量和数据治理的功能。此扩展包是 DataStage 的超集，并为以下方面提供无缝集成和一致的用户体验：

- 自动化数据发现
- 策略驱动的治理
- 自助式数据准备
- 对传输中和静态数据进行数据质量评估和清洗
- 高级动态或批处理导向的数据转换和移动

### 主数据管理

主数据是每笔业务交易、应用程序和决策的核心。用于 IBM Cloud Pak for Data 的 IBM **Master Data Connect** 服务提供 RESTful API，使地理分布的用户能够从 IBM **InfoSphere Master Data Management**（**InfoSphere MDM**）快速、可扩展且并发地访问您组织中最受信任的主数据。

InfoSphere MDM 充当中央存储库，用于跨组织管理、存储和维护主数据。InfoSphere MDM 提供以下功能：

- 组织关键业务事实的整合、集中视图
- 在主数据的整个生命周期内管理主数据的能力：

  ![](img/50e1dac0750b69f2b8e8efff000eb13b_167_0.png)

  图 7.8 – 主数据管理扩展包

InfoSphere MDM 11.6.0.11 或更高版本支持使用 IBM **MDM Publisher** 功能发布到 Master Data Connect。使用 MDM Publisher，您可以一次性批量加载主数据到 Master Data Connect，或设置持续的数据同步。当您在用户界面中配置 Master Data Connect 数据订阅时，MDM Publisher 会创建、运行并管理由此产生的数据加载作业。

加载组织的主数据后，Cloud Pak for Data 用户或应用程序可以使用强大的 API 直接访问可信数据。这使它们能够：

- 查看主数据记录。
- 对主数据执行全文或属性查询搜索。
- 对主数据运行图数据库查询。
- 查看主数据记录之间的关系。
- 查看数据的逻辑模型。

## 分析类扩展包 – IBM Palantir

Cloud Pak for Data 的核心用例之一是数据科学，基础平台包含一组丰富的功能，用于构建、部署、管理和治理由 IBM **Watson Studio** 支持的 AI 模型。此外，Cloud Pak for Data 还包含两个高级服务（也称为扩展包）来补充核心功能。

### IBM Palantir

用于 Cloud Pak for Data 的新的 IBM **Palantir** 扩展包将简化和加速组织如何将预测性、数据驱动的洞察注入各个行业领域的现实决策和业务运营中。IBM Palantir 服务有助于以下方面：

- 在易于部署和管理的“无代码、低代码”环境中提供面向业务的 AI 功能
- 简化数据和 AI 模型与业务决策的连接方式，同时具有可信性和可解释性
- 通过自动将数据映射到业务含义和行业上下文，为更明智的决策提供信息
- 自动化在混合云和数据环境中收集、组织和分析数据的方式

