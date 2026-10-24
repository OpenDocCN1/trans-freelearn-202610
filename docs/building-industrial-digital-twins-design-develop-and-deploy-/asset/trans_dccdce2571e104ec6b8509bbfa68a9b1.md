


阿里巴巴和腾讯主要在中国流行，因此，我们不会在此讨论这两个平台。我们将从数字孪生的角度，探讨当今行业中常见选项的功能。

# Microsoft Azure Digital Twins

Microsoft **Azure Digital Twins** (**ADT**) 是 Azure IoT 的一部分，是一个基于 IoT 的数字孪生平台，如在 _第 4 章，着手构建我们的第一个数字孪生_ 中所述。Azure IoT 包含多个平台服务，这些服务可以在边缘和云端访问。用于提供 IoT 平台的一些服务包括以下几项：

- **Azure IoT Hub**—这是一个云托管的、安全的双向通信服务，用于 IoT 设备与应用程序后端之间的通信。
- **Azure IoT Central**—这是一个 **用户界面 (UI)** 应用程序，用于管理 Azure IoT Hub 上的设备。
- **Azure IoT Edge**—这通过 Azure 服务的容器化运行 IoT 边缘设备。
- **Azure RTOS**—这是一个面向资源受限 IoT 设备的操作系统，包含嵌入式开发套件。
- **Azure Time Series Insights (TSI)**—使用此服务来存储、可视化和查询大量时间序列数据集。
- **ADT**—这是一个 **平台即服务 (PaaS)** 功能，用于为描述实体或事物模型及其与其他实体关系的实体创建基于图的关系。

这些服务根据业务和技术要求以不同配置组合，以提供 Azure IoT 平台。IoT 和数字孪生应用程序通过微软开发者工具（如 Visual Studio 和 **Visual Studio Code (VS Code)**）在 ADT 平台上开发。

ADT 中的知识图在单一模型中存储实体、关系和状态信息。_图 5.4_ 展示了 Azure IoT 平台及其服务的高级视图，以及 ADT 在架构中的位置。

ADT 使用 **数字孪生定义语言 (DTDL)**，我们在 _第 4 章，着手构建我们的第一个数字孪生_ 中已介绍过。DTDL 是一种开源建模语言，旨在提高 ADT 模型在不同应用程序和用例中的互操作性和可重用性。

您可以在此处查看架构概览：

![](img/f445915fceb4649a77b6797f1425850b_140_0.png)

图 5.4 – Azure IoT 平台中的 Microsoft ADT

微软在 Azure IoT 和 ADT 方面拥有广泛的行业关注度。这包括基础设施、制造、能源与公用事业、石油与天然气以及采矿等垂直领域（在撰写本书时）。

接下来我们将探讨 AWS 的功能。

# AWS IoT 平台

让我们来看看 AWS 的功能。AWS 早在 2006 年就开始提供公有云服务。如今，它在 **基础设施即服务 (IaaS)** 和 PaaS 市场中占有很大份额。2015 年底，Amazon 宣布了其 AWS IoT 产品，最初是 AWS IoT Core 或面向连接设备的云服务。其他 AWS 服务，例如 Amazon **简单存储服务 (S3)**、Amazon Kinesis 等，可以与 IoT Core 一起用于构建 IoT 应用程序。AWS IoT 平台支持常用的 IoT 协议，例如 **消息队列遥测传输 (MQTT)**、**超文本传输安全协议 (HTTPS)** 和 **低功耗广域网 (LoRaWAN)**。那些已经使用 AWS 进行 IaaS 和 PaaS 并将其企业数据和应用程序部署在 AWS 上的企业，开始考虑使用 AWS IoT 平台来开发需要设备连接和传感器数据的应用程序。

对于标准化于某个云平台的企业来说，它通常是新云功能（在此案例中为 IoT 和数字孪生）的起点。在某些情况下，CIO 希望在 IT 和云环境中拥有较少的组件。通常，公有云提供商的安全和合规审批过程很漫长，企业会尽量减少使用的公有云平台数量，除非他们愿意采用最佳方案解决方案，或将多云作为其业务连续性策略的一部分。通常，进出公有云平台会产生数据费用，并在应用程序之间引入延迟。

Amazon 的 IoT 产品仅限于其 IoT 平台。Amazon 未在平台之上构建任何可以作为软件即服务 (SaaS) 提供的 IoT 应用程序。SaaS 产品或 IoT 应用程序更易于业务用户使用，而平台功能通常需要软件专业人员和应用程序开发人员通过可用的 IoT 应用程序来使用这些服务。Amazon Sumerian 提供围绕三维 (3D) 建模、增强现实 (AR) 和虚拟现实 (VR) 的功能，可以与 IoT 平台一起用于构建类似数字孪生的应用程序。一个示例是制造工厂的数字孪生，带有设备和平面图的 3D 视图，用于为员工提供培训。

Amazon 提供设备影子服务（参见 https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html）。该服务允许您创建物理设备的影子，并例如将物理设备的状态提供给应用程序。但是，影子并不是物理资产的动态数字孪生。设备影子可以存储在 JavaScript 对象表示法 (JSON) 文件中。此类 JSON 文件中的信息将是设备元数据、状态或版本，以及与设备关联的任何安全令牌。应用程序可以使用表述性状态转移 (REST) 应用程序编程接口 (API)（即用于 GET、UPDATE 或 DELETE 请求）或通过 MQTT 消息与设备影子进行交互。

据我们所知，在 2021 年初撰写本书时，AWS 平台上没有完整的数字孪生服务。但是，有独立软件供应商 (ISV) 在 AWS 之上提供此类功能。一个示例是 Hexagon Digital Reality (HxDR) 平台（参见 https://aws.amazon.com/blogs/industries/hxdr-transforming-geospatial-data-in-the-cloud-with-aws-and-hexagon-leica-geosystems/）。

HxDR 提供用于在 AWS 之上构建结构的数字孪生功能。此类数字孪生解决方案的一些特征概述如下：

- 高计算复杂度，用于从物理结构中混合空间数据以创建数字孪生。随着模型规模和精度的增长，能够弹性增长计算能力的特点使公有云适合此用途。
- 高保真数字孪生需要大量关于其物理结构的数据。由于数据量大，存储并使这些数据可用是一项挑战性的任务。存储增长可以是按需或在公有云上进行。HxDR 使用 AWS S3 进行此类弹性存储。
- 数据可能会在全球范围内收集，具体取决于数字孪生的问题领域。通常，公有云平台在多个位置以及边缘位置都有接入点。
- 为数字孪生收集的数据的安全性和敏感性可能需要底层云平台提供额外的安全服务。

Vertex 是 AWS 平台上另一家提供数字孪生功能的供应商（参见 `https://aws.amazon.com/iot/solutions/VertexDigitalTwin/`）。Bosch IoT Things on AWS 可以使用 MQTT 和 **高级消息队列协议 (AMQP)** 连接到设备，并帮助管理数字孪生（参见 `https://aws.amazon.com/marketplace/pp/BoschIO-GmbH-Bosch-IoT-Things/B07DTJK8MV`）。

总结，我们看到像 AWS 这样的公有云提供商可以通过 IoT Core 和其他服务的组合，或来自 ISV 合作伙伴的专门产品，从零开始构建数字孪生功能，这些功能可以用于快速入门。但是，Amazon 缺乏其自身的完整数字孪生解决方案。

接下来让我们探讨 Oracle 在这一领域提供的功能。

# Oracle IoT 应用程序

让我们现在来看看 Oracle 的功能。Oracle 以 **企业资源计划 (ERP)** 和被全球多家中大型企业使用的技术解决方案而闻名。当一家公司寻找 IoT 和数字孪生解决方案时，当前的 ERP 和制造应用程序提供商可能会影响该决策。Oracle 和 SAP 是提供 ERP、**客户关系管理 (CRM)** 或 **人力资本管理 (HCM)** 等业务应用程序的两家主要全球供应商。该领域的其他供应商包括 `Salesforce.com`、Workday、Infor 等。一些大型全球公司可能拥有 Oracle、SAP 和 `Salesforce.com` 的组合，无论是由于不同的业务线还是并购活动。Oracle 的业务应用程序自 2012 年起通过云作为 SaaS 产品提供。Oracle 后来推出了其公有云平台，其当前产品称为 OCI。

