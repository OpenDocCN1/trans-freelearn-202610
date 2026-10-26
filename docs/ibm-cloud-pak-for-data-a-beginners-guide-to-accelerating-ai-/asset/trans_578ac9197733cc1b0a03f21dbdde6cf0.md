

AWS Quick Start 的其他优势包括 Cloud Pak for Data 最长 60 天的免费试用以及详细的部署指南。但是，它需要一个 AWS 账户——客户需承担基础设施成本。在存储方面，客户有两种选择：OpenShift Container Storage（OCS），最近更名为 OpenShift Data Foundations（ODF），或者 Portworx。

![](img/50e1dac0750b69f2b8e8efff000eb13b_144_0.png)

图 6.3 – Terraform 自动化

Terraform 自动化也可在 AWS、Azure 和 IBM Cloud 上使用，其优势包括：

-   轻松实现全栈部署，确保一致性和可重复性
-   通过基础设施即代码提高生产力，编写并执行代码，在几分钟内定义、部署和更新你的基础设施
-   幂等性，无论运行多少次，代码都能正确工作
-   通过自动化实现整个部署过程的自助服务，可按需调度和启动部署

## Azure Marketplace 和 QuickStart 模板

Azure Marketplace 是一个在线应用和服务市场，非常适合对 Cloud Pak for Data 软件及其服务感兴趣的 IT 专业人员和云开发人员。它让客户只需点击几下即可发现、试用、购买和部署解决方案。与 AWS 一样，Microsoft 要求拥有 Azure 账户，并从两种受支持的存储选项中选择一种，即 OCS 或 Portworx。此外，客户可以免费试用最长 60 天：

![](img/50e1dac0750b69f2b8e8efff000eb13b_145_0.png)

图 6.4 – Azure Marketplace 上的 Cloud Pak for Data

接下来，让我们介绍 Cloud Pak for Data as a Service。

## Cloud Pak for Data as a Service

IBM 的大多数数据和 AI 产品目前都以服务的形式提供，并打包在单一的 Cloud Pak for Data 订阅中，该订阅基于消费量，允许客户只为实际使用的部分付费。此外，为了加速现有工作负载的现代化并帮助客户迁移到云/托管服务，IBM 推出了一项名为 Hybrid Subscription Advantage（HSA）的计划，为现有软件客户提供使用 SaaS 而非软件的折扣。在本节中，我们将详细介绍 Cloud Pak for Data as a Service，包括可用的功能、定价和打包方式，以及 IBM 如何通过 HSA 帮助客户实现现代化。

如前所述，Cloud Pak for Data as a Service 允许客户现代化其收集、组织、分析和注入 AI 的方式，无需安装、管理或更新。换句话说，它帮助提供集成数据和 AI 平台的所有优势，而无需管理基础设施或软件的开销。构成 Cloud Pak for Data as a Service 的各项服务与 Cloud Pak for Data 软件相同，现有差距已在路线图中解决。以下是可用服务的简短列表及其描述和价值主张：

- 1. **IBM Watson Studio**：Watson Studio 使机器学习和深度学习大众化，加速 AI 注入你的业务以推动创新。Watson Studio 提供一套工具和一个协作环境，供数据科学家、开发人员和领域专家使用。它包括使用 IBM Watson Machine Learning 部署模型并大规模管理它们的能力，通过 Watson OpenScale 确保准确性和治理。最后但同样重要的是，它带有 Auto AI，使普通用户只需点击几下即可构建复杂模型。
  - a) **IBM Watson Machine Learning**：IBM Watson Machine Learning 是一项全服务的 IBM Cloud 产品，使开发人员和数据科学家能够轻松协作，将预测能力集成到他们的应用程序中。Machine Learning 服务是一组 REST API，你可以从任何编程语言调用这些 API 来开发应用程序，从而做出更明智的决策、解决棘手的问题并改善用户成果。
  - b) **IBM Watson OpenScale:** IBM Watson OpenScale 在其整个生命周期中跟踪和衡量 AI 的结果，并在不断变化的业务环境中调整和治理 AI。它还有助于处理漂移可解释性和偏差检测。

![](img/50e1dac0750b69f2b8e8efff000eb13b_147_0.png)

图 6.5 – Watson OpenScale

- 2. **IBM Watson Knowledge Catalog:** 使用 IBM Watson Knowledge Catalog 简化和组织你的企业数据，轻松实现大规模数据的查找、共享和治理。客户可以创建其数据的 360 度视图，无论数据存储在哪里（或采用何种格式），自动从众多受支持的数据源中发现数据，与其他用户协作，并通过定义策略和监控执行来控制数据访问。

- 3. **Data Virtualization** 使您能够跨多个系统查询数据，而无需复制和复制数据，从而节省时间并降低成本。Data Virtualization 从其源查询数据，通过提供最新和最准确的数据简化您的分析。Data Virtualization 服务会自动将您的数据节点组织到协作网络中，以提高计算效率。您可以使用大型或小型数据源定义星座。数据永远不会缓存在云或其他设备上。此外，您私有数据库的凭据会加密并存储在您的本地设备上。

- 4. **IBM Db2**：一个完全托管、高性能的关系型数据存储，运行企业级 Db2 数据库引擎，IBM Db2 旨在应对地球上最艰巨的关键任务工作负载，具有自适应工作负载管理、时间旅行查询、查询联合、数据库内 AI、行/列访问控制、审计以及支持 JSON、XML 和地理空间数据集等高级功能。客户可以独立扩展和管理其部署的计算和存储需求。通过自助式托管备份到对象存储并支持时间点恢复，可以将数据恢复到任何指定时间，同时所有客户数据在传输和静态时均加密。
  - a) **IBM Db2 Warehouse**：一个完全托管的弹性云数据仓库，可独立扩展存储和计算。它提供高度优化的列式数据存储、可操作的压缩和内存处理，以增强您的分析和机器学习工作负载。

- 5. **IBM DataStage** 提供业界领先的批量和实时数据集成，可在本地和混合云环境中构建可信的数据管道，允许任何集成风格（ETL 或 ELT）为 AI 准备数据。请注意，ETL 代表 **Extract, Transform, Load**（提取、转换、加载），而 ELT 是先提取和加载，然后进行处理。与您的同事一起使用 DataStage Flows，并控制对项目的管理员、编辑者或查看者访问权限。在友好的用户界面中，轻松在无代码/低代码环境中进行数据集成工作。在安全的云环境中根据需要水平或垂直扩展。利用共享平台连接以及与 Cloud Pak for Data 中其他产品的集成。

- 7. **IBM Analytics Engine** 帮助使用开源 Apache Spark 和 Apache Hadoop 开发和部署分析应用程序。使用您自己的分析库和开源包自定义集群。与 IBM Watson Studio 或第三方应用程序集成，向集群提交作业。达拉斯区域为标准小时计费和标准月度计划提供 HIPAA 就绪选项。通过选择合适的软件包、版本和集群大小，根据应用程序的要求定义集群。在需要时使用集群，并在所有作业运行完毕后再次删除它。使用第三方分析库和包自定义集群，并从 IBM Watson（如 IBM Watson Studio 和 Machine Learning）部署工作负载。基于符合 ODPi 的 Apache Spark 和 Apache Hadoop 技术栈构建，以扩展开源投资。使用标准的开源 API 和库集成分析工具。

![](img/50e1dac0750b69f2b8e8efff000eb13b_150_0.png)

图 6.8 – IBM Analytics Engine

- 8. **IBM Match 360 with Watson** 帮助使用主数据快速构建用于分析和其他数据科学用例的数据管道。从您的 IBM MDM Advanced 或 Standard Edition 实体开始，或从知识目录中包含参与方信息的任何数据资产开始，快速将新属性映射并建模到您的数据模型中，以获得更完整的客户视图。AI 驱动的匹配引擎利用客户信赖的统计方法加速配置，以产生准确的结果。结果可通过 RESTful API 访问，导出为平面文件，或通过实体资源管理器在线查看。使用受治理的数据资产扩展 MDM Advanced 或 Standard Edition 中的现有主数据实体。

