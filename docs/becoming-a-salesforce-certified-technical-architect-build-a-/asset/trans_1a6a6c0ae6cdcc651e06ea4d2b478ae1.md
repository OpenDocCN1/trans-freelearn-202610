


相应地更新你的数据模型图。使用业务流程图会更容易解释这个业务流程的解决方案。该图可能如下所示：

![](img/a3b1eecf3c87c814267f2877e8a8be1a_491_0.png)

图 14.14 – 客户注册业务流程图——邀请子流程

至此，该业务流程结束。进入第六个也是最后一个业务流程。

## 现场销售

PLU 共享了现场销售流程的两个关键需求。逐一分析并解决它们。从第一个需求开始，它以如下内容开头：

现场销售代理人访问潜在的 B2B 线索，并向其介绍不同的产品方案。

你在此场景中之前遇到过类似的需求，但渠道不同。你决定在 MuleSoft 暴露的 API 基础上开发自定义 UI，这些 API 与 Power Sales 集成。该 UI 基本上是一组 **Lightning 组件**。

现场销售代理人可以在他们的平板电脑上使用相同的组件，前提是他们使用 **Salesforce 移动应用**。新版 Salesforce 移动应用支持 Lightning 应用页面，你可以合理推断自定义 UI 将使用这些页面。

更新你的架构图和接口列表，然后进入下一个需求，它以如下内容开头：

*访问完成后三天，应向 B2B 客户的主要联系人发送电子邮件调查。*

应使用两个不同的调查模板，一个用于成功签约的交易，另一个用于丢失的交易。如果 **现场销售** 代理人的评分低于 10 分中的 3 分，应自动创建一个 **案例** 并分配给现场销售代理人的经理。

如今，这一要求越来越常见，因为越来越多的公司试图更贴近客户，并向他们表明他们的意见很重要。你有三种潜在方法可以解决这一要求：

- 基于自定义对象和自动化开发自定义调查模块。
- 使用诸如 **SurveyMonkey** 之类的第三方工具。
- 使用 **Salesforce 调查**。

Salesforce 产品附带 Salesforce 的常规承诺，即它们会定期更新，并会持续加入新功能。此外，它们是专为 Salesforce 设计的产品，因此与其他产品相比，它们与平台的集成方式更加直接，并且通常简化其他非功能性要求，例如数据驻留。

在某些情况下，Salesforce 产品可能缺乏某些功能，这证明考虑第三方替代方案是合理的。此处并非这种情况。所述要求可以使用 Salesforce 调查来满足。

与其他扩展产品（例如 **Salesforce 现场服务** 和 **Vlocity**）类似，你需要知道产品的工作原理并很好地理解其组件。你最需要熟悉的常见元素是数据模型。将你计划从 Salesforce 调查中使用的关键对象添加到数据模型图中。

**注意**
你可以在以下链接中找到更多信息：`https://packt.link/DWKUy`。

你仍需要添加一些自定义以提供某些功能，例如安排发送调查、将发送的调查链接到正确的对象（在此示例中为线索），以及处理反馈以确定是否需要创建案例并将其分配给现场销售代理人的经理（如果现场销售代理人的评分低于 10 分中的 3 分）。

如果使用 Salesforce 流开发，此自动化可能会变得复杂。因此，你可以提出一种基于 Apex 的解决方案。

> 注意
> 使用 Apex 而非 Salesforce 流来交付复杂逻辑更为合适。极其复杂的流比 Apex 更难调试或阅读。流仍然非常适合解决许多其他挑战，但仍不完全是 Apex 的替代品。

更新你的数据模型图。你的架构图现在可能如下所示：

![](img/a3b1eecf3c87c814267f2877e8a8be1a_493_0.png)

你的集成接口列表可能如下所示：

| 接口代码 | 源/目标 | 集成层 | 集成模式 | 描述 | 安全 | 身份验证 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Int-001 | Salesforce <-> Marketing Cloud | 数据 | 批量数据同步 – 异步 | 使用 Marketing Cloud 连接器同步 Salesforce 与 Marketing Cloud 之间的联系人和线索等数据。 | HTTPS (单向 SSL/TLS) | OAuth 2.0 客户端凭据流（用于向 Marketing Cloud 进行身份验证）。OAuth 2.0 Web 服务器流（用于 MuleSoft 托管的集成流）。 |
| Int-002 | Salesforce -> MuleSoft -> Power Sales | 业务逻辑 | 远程进程调用 – 请求和响应 – 阻塞 | 调用多个方法并从 Power Cloud 获取报价详细信息。API 可以在 Salesforce 的自定义 UI 中使用按钮调用。单击按钮后，将调用一个暴露给外部世界的 Apex 控制器。MuleSoft 将确定是调用 Power Sales 的某个 API 还是直接连接到其数据库以在内部执行逻辑。将触发一个集成流，Apex 控制器创建 Salesforce 报价记录并将其链接到相关机会。 | Salesforce 和 MuleSoft 之间使用 HTTPS (双向 SSL/TLS)。MuleSoft 和 Power Sales 之间使用 HTTPS (单向 SSL/TLS) | Salesforce 和 MuleSoft 之间使用带有相互身份验证的 OAuth 2.0 Web 服务器流（随后是刷新令牌流）。MuleSoft 和 Power Sales 之间使用简单身份验证。 |
| Int-003 | Salesforce -> MuleSoft -> The Right ERP | 数据 | 远程进程调用 – 发送后忘记 | 从 Salesforce 触发平台事件。这将消耗 MuleSoft 暴露的数据。MuleSoft 将使用平台事件中的数据来确定将数据发送到哪个 ERP。 | HTTPS (单向 SSL/TLS) | OAuth 2.0 JWT 持有者令牌流。 |
| Int-004 | ERPs (比利时除外) -> MuleSoft -> Salesforce | 数据 | 远程调用 | ERPs 将向 MuleSoft 发送消息以指示切换过程的完成。MuleSoft 随后将调用标准 Salesforce REST API 来更新相关记录。 | ERPs 和 MuleSoft 之间使用 HTTPS (双向 SSL/TLS)。MuleSoft 和 ERPs 之间使用 HTTPS (单向 SSL/TLS) | Salesforce 和 MuleSoft 之间使用带有相互身份验证的 OAuth 2.0 Web 服务器流（随后是刷新令牌流）。MuleSoft 和 ERPs 之间使用简单身份验证。 |
| Int-005 | 比利时 ERP -> Salesforce | 数据 | 远程调用 (电子邮件: smtp) | 比利时 ERP 使用 SMTP 服务器发送的电子邮件，由 Salesforce 自定义电子邮件服务接收。 | HTTPS (单向 SSL/TLS) | 无 (对 Salesforce)。与 SMTP 服务器进行简单身份验证。 |
| Int-006 | MuleSoft -> Salesforce | UI | 数据可视化/混搭/ODA | 使用 Salesforce Connect 在 Salesforce 中显示发票、发票行项目和付款。发票将数据触发到相关 ERPs，并使用 OData 接口对其进行聚合。 | HTTPS (单向 SSL/TLS) | OAuth 2.0 Web 服务器流（随后是刷新令牌流）。 |
| Int-007 | MuleSoft -> ERPs MuleSoft -> PDF Generator | 业务逻辑 | 远程进程调用 – 请求和响应 – 阻塞 | 定时 MuleSoft 作业检索新的发票数据并将其传递给 PDF Generator 以生成 PDF 发票。 | HTTPS (单向 SSL/TLS) | 简单身份验证。 |
| Int-008 | MuleSoft -> SFTP MuleSoft -> Salesforce | 数据 | 批量数据同步 – 异步 | 定时 MuleSoft 作业从 SFTP 检索 PDF 发票并将其传输到 Salesforce。 | Salesforce 和 MuleSoft 之间使用 HTTPS (双向 SSL/TLS)。MuleSoft 和 SFTP 之间使用 HTTPS (单向 SSL/TLS) | Salesforce 和 MuleSoft 之间使用带有相互身份验证的 OAuth 2.0 Web 服务器流（随后是刷新令牌流）。MuleSoft 和 SFTP 之间使用简单身份验证。 |
| Int-009 | Salesforce -> MuleSoft | 数据 | 远程进程调用 – 发送后忘记 | 从 Salesforce 为任何新创建的关键事件 (案例) 触发平台事件。MuleSoft 将订阅此事件。MuleSoft 将使用平台事件中的数据查询 Salesforce 以获取更多详细信息。 | HTTPS (单向 SSL/TLS) | OAuth 2.0 JWT 持有者令牌流。 |
| Int-010 | MuleSoft -> Salesforce MuleSoft -> Marketing Cloud | 业务逻辑 | 远程进程调用 – 请求和响应 – 阻塞 | MuleSoft 查询 Salesforce 中受关键事件 (案例) 影响的客户记录。MuleSoft 将在 Salesforce 中创建案例以发布事务性消息 API 并向这些客户发送 SMS 通知。 | Salesforce 和 MuleSoft 之间使用 HTTPS (双向 SSL/TLS)。MuleSoft 和 Marketing Cloud 之间使用 HTTPS (单向 SSL/TLS) | OAuth 2.0 Web 服务器流（随后是刷新令牌流）。 |
| Int-011 | Salesforce 现场服务移动应用 -> Salesforce | 数据 | 远程调用 | 移动应用将调用 REST API 来查询、创建或更新记录。 | HTTPS (单向 SSL/TLS) | OpenID Connect 用户代理流（随后是刷新令牌流）。 |
| Int-012 | 自定义移动应用 -> Salesforce | 数据 | 远程调用 | 移动应用通过 Bluetooth 与 Radar 读取器通信，读取值，创建客户，然后使用 REST API 将其传输到 Salesforce。 | HTTPS (单向 SSL/TLS) | OpenID Connect 用户代理流（随后是刷新令牌流）。 |
| Int-013 | MuleSoft -> 智能电表平台 MuleSoft -> Salesforce | 数据 | 远程调用 | 定时 MuleSoft 作业从不同平台检索表计读数。然后将数据转换为统一结构并合并。合并后的数据集随后用于更新 Salesforce。 | Salesforce 和 MuleSoft 之间使用 HTTPS (双向 SSL/TLS)。MuleSoft 和 Salesforce 之间使用 HTTPS (单向 SSL/TLS) | Salesforce 和 MuleSoft 之间使用带有相互身份验证的 OAuth 2.0 Web 服务器流（随后是刷新令牌流）。MuleSoft 和 Salesforce 之间使用简单身份验证。 |
| Int-014 | Salesforce 移动应用 (通过 iOS Mobile Publisher) -> Salesforce | 数据 | 远程调用 | 移动应用将调用 REST API 来查询、创建或更新记录。 | HTTPS (单向 SSL/TLS) | OpenID Connect 用户代理流（随后是刷新令牌流）。 |
| Int-015 | Salesforce 移动应用 -> Salesforce | 数据 | 远程调用 | 移动应用将调用 REST API 来查询、创建或更新记录。 | HTTPS (单向 SSL/TLS) | OpenID Connect 用户代理流（随后是刷新令牌流）。 |

图 14.16 – 集成接口 (第二稿)
