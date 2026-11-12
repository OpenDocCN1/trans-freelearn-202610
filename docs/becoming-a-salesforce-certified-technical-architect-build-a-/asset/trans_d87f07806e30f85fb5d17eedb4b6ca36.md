

- **操作层**：此模式在数据层上运行。
- **关键用例**：将一个系统中的一个数据集复制到另一个系统，或同步两个不同系统上的两个数据集。由于其高效性，这种模式很常见，尽管它无法满足当今应用程序所要求的实时/准实时体验。在许多情况下，业务可以接受延迟的数据同步，例如每 12 小时将发票从开票系统复制到数据仓库。
- **相关的 Salesforce 功能**：Salesforce Bulk API 是一个相关功能（适用于较小数据量的 REST、SOAP 和 Graph API 也是如此）。Salesforce Batch Apex 结合回调可能很诱人，但这是一种有风险的模式，建议您避免使用。请考虑改用中间件。

> 注意
> 您可以在以下链接中找到更多详细信息：https://packt.link/B5qRq.

## 远程调用

以下是有关此模式的高级信息：

- **描述**：当远程系统想要在 Salesforce 中调用一个流程时使用此模式。从远程系统的角度来看，这将是 RPI 模式之一。从 Salesforce 的角度来看，它被视为调用。
- **时机**：此模式可以是同步的或异步的，具体取决于实现（调用 Salesforce 的外部系统可能会等待响应，也可能不会）。
- **操作层**：此模式在数据层和业务逻辑层上运行，具体取决于实现。
- **关键用例**：调用标准 Salesforce API 以创建、读取、更新或删除（CRUD）数据，或调用自定义 Salesforce Web 服务（SOAP 或 REST），以返回特定结果。
- **相关的 Salesforce 功能**：多个 Salesforce 功能相关，例如 Salesforce REST API、Salesforce SOAP API、Salesforce Metadata API、Salesforce Tooling API、Salesforce Bulk API（用于批量操作）和 Apex Web 服务。

值得注意的是，当您希望暴露一个应用全有或全无事务原则的原子功能时，Apex Web 服务特别有用，例如，暴露一个接受特定数据结构作为输入并生成客户、机会和机会产品行的 Web 服务。如果创建这些对象中的任何记录失败，将导致整个事务回滚，当数据完整性是关注点时，此类原子事务至关重要。

您还可以利用 Salesforce 标准 REST API 提供的复合请求来创建原子事务。在复合请求中，多个依赖请求在单个调用中执行，一个请求的输出可能成为后续请求的输入。您可以指定，在子请求中遇到的错误是导致整个复合请求完整回滚，还是仅回滚依赖于它的子请求。

> 注意
> 您可以在以下链接中找到更多详细信息：https://packt.link/K596j.

## 基于数据变更的 UI 更新

以下是有关此模式的信息：

- **描述**：这是一个相对较新的模式。当您需要根据 Salesforce 数据的某些变更更新 Salesforce UI，而无需刷新整个页面时使用它。
- **时机**：此模式是异步的。
- **操作层**：此模式在 UI 层上运行。
- **关键用例**：构建一个基于 Salesforce 数据显示特定值和图表的动态页面，并在基础数据变更时更新该页面的某些元素。
- **相关的 Salesforce 功能**：多个 Salesforce 功能相关，例如 Salesforce Streaming API、推送通知、平台事件和 CDC（技术上基于平台事件）。

> 注意
> 您可以在以下链接中找到更多详细信息：https://packt.link/S4Ql1.

## 数据虚拟化

以下是有关此模式的信息：

- **描述**：此模式用于在 Salesforce 中检索和显示存储在远程系统上的数据，而无需持久化数据。当无需将数据保留在 Salesforce 中时，这非常有用。数据会实时按需获取并显示给用户。
- **时机**：此模式是同步的。但是，诸如懒加载等某些技术可以将其转变为异步通信的一种特殊情况。
- **操作层**：此模式在 UI 层上运行。
- **关键用例**：构建利用 Salesforce 外部托管数据的 UI，例如归档数据。使用 Google 地图等远程服务的混搭应用是另一个有效的用例。
- **相关的 Salesforce 功能**：自定义 Visualforce 页面、Lightning 组件、Salesforce Connect 和 Salesforce Canvas 都是相关功能。

在设计集成接口时，您需要定义模式并指定要使用的 Salesforce 技术。这项技能是您在此领域接下来需要掌握的内容。

## 选择并证明合适的平台特定集成能力

Salesforce 提供了一组开箱即用的功能，以促进不同的集成模式：

| 功能 | 描述 |
|---|---|
| REST API | Salesforce 自带开箱即用的 REST API，可用于与 Salesforce 数据交互。一般来说，REST API 易于集成，尤其是来自移动端和 JavaScript。Salesforce REST API 支持简单的 HTTP 调用（GET、POST、PUT、PATCH 和 DELETE）。 |
| SOAP API | Salesforce 自带的开箱即用 SOAP API 可以通过使用标准的两个 Web 服务描述语言 (WSDL) 文件之一来使用。企业 WSDL 是强类型的，这使得维护具有挑战性。但是，如果数据模型很少更改，则使用它更容易。另一方面，合作伙伴 WSDL 是弱类型的，这使其更加动态，适合由合作伙伴或经常遇到数据模型更改的组织使用。SOAP API 可用于像 REST API 一样处理 Salesforce 数据。但是，对于移动设备和 JavaScript 应用等瘦客户端来说，其实现更具挑战性。此外，它的负载往往更大，因此被认为比 REST 稍慢。 |
| Bulk API | 此 API 旨在处理大量数据。某些 ETL 工具设计为在用于向 Salesforce 加载大量数据时自动切换到此 API。 |
| Salesforce Connect | 这是一项付费功能，允许从支持 OData 2.0 或 OData 4.0 标准的外部数据源实时检索数据。OData 接口是 REST 风格的，您可以使用自定义代码调用它们。Salesforce Connect 使该过程更加简单。外部 OData 表会自动在 Salesforce 中表示为外部对象，从而允许将此数据与其他 Salesforce 数据关联，并使用常规 SOQL 语句进行查询。 |
| Canvas | Salesforce Canvas 是一项付费功能，可让您在 Salesforce 中集成远程 Web 应用程序。这通常是 UI 级集成，但 Canvas SDK 可以添加更高级的功能。 |
| Chatter REST API | Chatter 有其自己的 REST API。此 API 可以与 Chatter 信息流、推荐、群组、话题和关注者等对象进行交互。 |
| Metadata API | 此 API 不太可能用于数据集成场景。它主要由发布管理工具使用。它用于检索、创建、更新、部署或删除 Salesforce 元数据信息，例如页面布局和用户配置文件。 |
| Streaming API | 流 API 用于开发现代动态页面。它允许您的页面/组件在 Salesforce 数据更改时接收通知，从而相应地更新页面/组件的部分内容。它利用 PushTopic 功能。 |
| Tooling API | 此 API 旨在帮助构建与 Salesforce 平台交互的自定义开发工具。 |
| Apex callouts | 此功能允许使用 Apex 调用远程 Web 服务。Apex 调用可以是同步或异步的。异步方法必须带有 @future 注解。您还可以利用 Queueable 接口来开发带有异步方法的 Apex 类。您可以使用 continuation 类进行长时间运行的调用。 |
| Email | 这是一种旧的集成技术。但是，它仍然有效并有其用例，例如在与不支持任何较新接口的旧版应用程序集成时。您可以利用 InboundEmailHandler 接口来开发自定义 Apex 类，以处理 Salesforce 电子邮件服务接收到的入站电子邮件。 |

