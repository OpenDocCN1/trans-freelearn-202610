

启用对主数据实体的文本搜索和查询。使用 RESTful API 或 IBM App Connect 构建服务集成，以便在业务应用程序或数据仓库中使用。自动对字段进行分类并映射到您的数据模型，并使用以数据为先、AI 建议的匹配属性来优化算法。内置机器学习和多年的经验可加快配置和调整实体匹配算法的速度。

- 9. **Watson Assistant：** 将对话界面构建到任何应用程序、设备或渠道中。为您的应用程序添加自然语言界面，以自动化与终端用户的交互。常见应用包括虚拟代理和聊天机器人，可在任何渠道或设备上集成和通信。通过易于使用的 Web 应用程序训练 Watson Conversation 服务，该应用程序旨在让您能够快速构建应用程序与用户之间的自然对话流程，并部署可扩展、经济高效的解决方案。

  ![](img/50e1dac0750b69f2b8e8efff000eb13b_151_0.png)

  Figure 6.9 – IBM Watson Assistant

  - a) **Speech to Text** 服务将人类语音输入转换为文本。该服务使用深度学习 AI 应用语法、语言结构以及音频和语音信号的组成知识，以准确转录人类语音。它可用于语音自动聊天机器人、客户服务呼叫中心的分析工具以及多媒体转录等多种应用。
  - b) **Text to Speech** 服务将书面文本转换为自然听感的语音。该服务以最小延迟流式传输合成的音频。音频使用其语言和方言的适当节奏和语调，提供流畅自然的声音。该服务可用于语音自动聊天机器人，以及各种语音驱动和无屏幕应用，例如残疾人或视障人士工具、视频旁白和配音，以及教育和家庭自动化解决方案。
  - c) **Natural Language Understanding** 有助于分析文本并从内容中提取元数据，例如概念、实体、关键词、类别、情感、情绪、关系和语义角色。使用 Watson Knowledge Studio 开发的自定义注释模型，可在非结构化文本中识别行业/领域特定的实体和关系。

- 10. **Watson Discovery** 为应用程序添加认知搜索和内容分析引擎，以识别模式、趋势和可操作的洞察，从而推动更好的决策制定。它安全地统一结构化和非结构化数据，并使用预丰富内容和简化的查询语言，消除手动筛选结果的需求。完全托管的云服务中的自动化摄取和集成自然语言处理消除了处理自然语言内容的复杂性。通过使用高级开箱即用的 AI 功能，例如自然语言查询、段落检索、相关性训练、关系图和异常检测，来发现数据中的深层联系。

- 11. **开源数据库：**

  - a) **Cloudant** 是一种完全托管的 JSON 文档数据库，提供预配吞吐量容量和存储的独立无服务器扩展。Cloudant 与 Apache CouchDB 兼容，并可通过简单易用的 HTTPS API 访问，适用于 Web、移动和物联网应用。
  - b) **Elasticsearch** 将全文搜索引擎的强大功能与 JSON 文档数据库的索引优势相结合，创建了用于大量数据丰富分析的强大工具。IBM Cloud Databases for Elasticsearch 通过为您管理一切，使 Elasticsearch 变得更好。
  - c) **EDB** 是基于 PostgreSQL 的数据库引擎，针对性能、开发人员生产力和与 Oracle 的兼容性进行了优化。IBM Cloud Databases for EDB 是一种完全托管的服务，提供 24/7 运营和支持。
  - d) **MongoDB** 是一种 JSON 文档存储，具有丰富的查询和聚合框架。IBM Cloud Databases for MongoDB 通过为您管理一切，使 MongoDB 变得更好。
  - e) **PostgreSQL** 是一种功能强大、开源的对象关系数据库，具有高度可定制性。它是一款功能丰富的企业数据库，支持 JSON，让您兼得 SQL 和 NoSQL 世界的优势。IBM Cloud Databases for PostgreSQL 通过为您管理一切，使 PostgreSQL 变得更好。

## Packaging and pricing

Cloud Pak for Data as a Service 以订阅方式提供，客户按需付费，不多付费。此外，订阅积分可用于构成 Cloud Pak for Data 的任何数据和 AI 服务。这为客户提供了很大的灵活性，因为他们可以配置和使用所有范围内的服务。这些单独的服务均以具有竞争力的价格定价，以反映其提供的价值，并包含计算、内存、存储和网络等基础设施成本。

此外，IBM 还有一个名为 HSA 的有吸引力的计划，以激励现有本地客户将业务现代化到 Cloud Pak for Data as a Service。该计划的价值在于，现有软件客户将获得 Cloud Pak for Data as a Service 的大幅折扣，以抵消他们已支付的软件价值。当然，这假设客户将停止使用其他软件，转而使用 Cloud Pak for Data as a Service。

## IBM Cloud Satellite

IBM Cloud Satellite 是 IBM Cloud 的扩展，可在客户数据中心、边缘服务器或任何云基础设施上运行。IBM 已将所有云服务移植到 Kubernetes，使不同服务能够一致运行。IBM Cloud Satellite 扩展并利用了相同的底层概念，以 Red Hat OpenShift 作为其 Kubernetes 管理环境运行。

更准确地说，每个 Cloud Satellite 位置都是在本地硬件或任何第三方公有云（如 AWS 或 Azure）上运行的 IBM Cloud 实例。此外，每个 Cloud Satellite 位置都连接到 IBM Cloud 控制平面。该连接回 IBM Cloud 控制平面提供审计、数据包捕获和对安全团队的可见性，以及跨所有卫星位置的应用程序和服务的全局视图。IBM Cloud Satellite Link 将 IBM Cloud 连接到其卫星位置，并提供进出所有流量的可见性。

IBM 的战略是利用 Cloud Satellite 将 Cloud Pak for Data 托管服务带到第三方云和专用本地基础设施。截至 2021 年，一部分 Cloud Pak for Data 托管服务，包括 Jupyter Notebook 和 DataStage，已在 Cloud Satellite 上提供，可部署在第三方云上。这些服务的打包和定价与 IBM Cloud 上的方式没有区别，并且将包含运行服务的第三方云基础设施。最终目标是在您选择的<|message|> 基础设施上交付托管服务。

![](img/50e1dac0750b69f2b8e8efff000eb13b_154_0.png)

Figure 6.10 – IBM Cloud Satellite

那么，这一切是如何运作的？一旦建立了 Satellite 位置，这些 Satellite 位置中的所有工作负载都在 Red Hat OpenShift 上运行——具体来说是 IBM Cloud 的托管 OpenShift 服务。集成到 Cloud Pak for Data as a Service 中的服务——例如 DataStage——可以将运行时部署到这些 Satellite 位置，以便这些运行时更接近服务需要集成的[content]

启用对主数据实体的文本搜索和查询。使用 RESTful API 或 IBM App Connect 构建服务集成，以便在业务应用程序或数据仓库中使用。自动对字段进行分类并映射到您的数据模型，并使用以数据为先、AI 建议的匹配属性来优化算法。内置机器学习和多年的经验可加快配置和调整实体匹配算法的速度。

- 9. **Watson Assistant：** 将对话界面构建到任何应用程序、设备或渠道中。为您的应用程序添加自然语言界面，以自动化与终端用户的交互。常见应用包括虚拟代理和聊天机器人，可在任何渠道或设备上集成和通信。通过易于使用的 Web 应用程序训练 Watson Conversation 服务，该应用程序旨在让您能够快速构建应用程序与用户之间的自然对话流程，并部署可扩展、经济高效的解决方案。

  ![](img/50e1dac0750b69f2b8e8efff000eb13b_151_0.png)

  Figure 6.9 – IBM Watson Assistant

  - a) **Speech to Text** 服务将人类语音输入转换为文本。该服务使用深度学习 AI 应用语法、语言结构以及音频和语音信号的组成知识，以准确转录人类语音。它可用于语音自动聊天机器人、客户服务呼叫中心的分析工具以及多媒体转录等多种应用。
  - b) **Text to Speech** 服务将书面文本转换为自然听感的语音。该服务以最小延迟流式传输合成的音频。音频使用其语言和方言的适当节奏和语调，提供流畅自然的声音。该服务可用于语音自动聊天机器人，以及各种语音驱动和无屏幕应用，例如残疾人或视障人士工具、视频旁白和配音，以及教育和家庭自动化解决方案。
  - c) **Natural Language Understanding** 有助于分析文本并从内容中提取元数据，例如概念、实体、关键词、类别、情感、情绪、关系和语义角色。使用 Watson Knowledge Studio 开发的自定义注释模型，可在非结构化文本中识别行业/领域特定的实体和关系。

- 10. **Watson Discovery** 为应用程序添加认知搜索和内容分析引擎，以识别模式、趋势和可操作的洞察，从而推动更好的决策制定。它安全地统一结构化和非结构化数据，并使用预丰富内容和简化的查询语言，消除手动筛选结果的需求。完全托管的云服务中的自动化摄取和集成自然语言处理消除了处理自然语言内容的复杂性。通过使用高级开箱即用的 AI 功能，例如自然语言查询、段落检索、相关性训练、关系图和异常检测，来发现数据中的深层联系。

- 11. **开源数据库：**

  - a) **Cloudant** 是一种完全托管的 JSON 文档数据库，提供预配吞吐量容量和存储的独立无服务器扩展。Cloudant 与 Apache CouchDB 兼容，并可通过简单易用的 HTTPS API 访问，适用于 Web、移动和物联网应用。
  - b) **Elasticsearch** 将全文搜索引擎的强大功能与 JSON 文档数据库的索引优势相结合，创建了用于大量数据丰富分析的强大工具。IBM Cloud Databases for Elasticsearch 通过为您管理一切，使 Elasticsearch 变得更好。
  - c) **EDB** 是基于 PostgreSQL 的数据库引擎，针对性能、开发人员生产力和与 Oracle 的兼容性进行了优化。IBM Cloud Databases for EDB 是一种完全托管的服务，提供 24/7 运营和支持。
  - d) **MongoDB** 是一种 JSON 文档存储，具有丰富的查询和聚合框架。IBM Cloud Databases for MongoDB 通过为您管理一切，使 MongoDB 变得更好。
  - e) **PostgreSQL** 是一种功能强大、开源的对象关系数据库，具有高度可定制性。它是一款功能丰富的企业数据库，支持 JSON，让您兼得 SQL 和 NoSQL 世界的优势。IBM Cloud Databases for PostgreSQL 通过为您管理一切，使 PostgreSQL 变得更好。

## Packaging and pricing

Cloud Pak for Data as a Service 以订阅方式提供，客户按需付费，不多付费。此外，订阅积分可用于构成 Cloud Pak for Data 的任何数据和 AI 服务。这为客户提供了很大的灵活性，因为他们可以配置和使用所有范围内的服务。这些单独的服务均以具有竞争力的价格定价，以反映其提供的价值，并包含计算、内存、存储和网络等基础设施成本。

此外，IBM 还有一个名为 HSA 的有吸引力的计划，以激励现有本地客户将业务现代化到 Cloud Pak for Data as a Service。该计划的价值在于，现有软件客户将获得 Cloud Pak for Data as a Service 的大幅折扣，以抵消他们已支付的软件价值。当然，这假设客户将停止使用其他软件，转而使用 Cloud Pak for Data as a Service。

## IBM Cloud Satellite

IBM Cloud Satellite 是 IBM Cloud 的扩展，可在客户数据中心、边缘服务器或任何云基础设施上运行。IBM 已将所有云服务移植到 Kubernetes，使不同服务能够一致运行。IBM Cloud Satellite 扩展并利用了相同的底层概念，以 Red Hat OpenShift 作为其 Kubernetes 管理环境运行。

更准确地说，每个 Cloud Satellite 位置都是在本地硬件或任何第三方公有云（如 AWS 或 Azure）上运行的 IBM Cloud 实例。此外，每个 Cloud Satellite 位置都连接到 IBM Cloud 控制平面。该连接回 IBM Cloud 控制平面提供审计、数据包捕获和对安全团队的可见性，以及跨所有卫星位置的应用程序和服务的全局视图。IBM Cloud Satellite Link 将 IBM Cloud 连接到其卫星位置，并提供进出所有流量的可见性。

IBM 的战略是利用 Cloud Satellite 将 Cloud Pak for Data 托管服务带到第三方云和专用本地基础设施。截至 2021 年，一部分 Cloud Pak for Data 托管服务，包括 Jupyter Notebook 和 DataStage，已在 Cloud Satellite 上提供，可部署在第三方云上。这些服务的打包和定价与 IBM Cloud 上的方式没有区别，并且将包含运行服务的第三方云基础设施。最终目标是在您选择的 infrastrukture 上交付托管服务。

![](img/50e1dac0750b69f2b8e8efff000eb13b_154_0.png)

Figure 6.10 – IBM Cloud Satellite

那么，这一切是如何运作的？一旦建立了 Satellite 位置，这些 Satellite 位置中的所有工作负载都在 Red Hat OpenShift 上运行——具体来说是 IBM Cloud 的托管 OpenShift 服务。集成到 Cloud Pak for Data as a Service 中的服务——例如 DataStage——可以将运行时部署到这些 Satellite 位置，以便这些运行时更接近服务需要集成的 data 或应用。名为 Satellite Link 的安全连接在 Cloud Pak for Data as a Service 与其远程运行时之间提供通信。

IBM Cloud Satellite 对数据和 AI 工作负载的价值主张有三点：

- 1. **Data gravity**：最小化数据移动和冗余，这也有助于避免出口成本。在数据所在位置附近训练 AI 模型，可消除延迟并提高性能。
- 2. 现代化传统工作负载，并在本地或您选择的第三方云上实现云原生优势：
  - 易于配置并按需扩展。
  - 无缝升级，停机时间可忽略不计。
  - 实现托管服务的优势。
- 3. 解决监管和合规挑战：
  - 在不移动数据的情况下共享洞察：处理数据主权挑战。
  - 符合 GDPR、CCPA 等。

现在我们已经了解了 IBM Cloud Satellite 的基础知识和 IBM 的多云方法，让我们来看一下 **data fabric**。

## A data fabric for a multi-cloud future

云正在改变每个企业，多云是未来。为了取得成功，企业将必须在私有云和公有云之间访问、治理、保护、转换和管理数据。这就是 IBM 使用数据 fabric 所解决的问题，这是一项重大努力，可能需要数年时间。数据 fabric 的坚实基础始于集中式元数据管理、数据治理和数据隐私，当通过自动化增强时，确实有助于放大其优势。

