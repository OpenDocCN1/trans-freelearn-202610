
现在，请查看景观架构图：

![](img/a3b1eecf3c87c814267f2877e8a8be1a_198_0.png)

## 图 5.6 – 景观架构（最终）

## 你的集成接口表现在将如下所示：

| 接口代码 | 源/目标 | 集成层 | 集成模式 | 描述 | 安全 | 身份验证 |
|---|---|---|---|---|---|---|
| Int-001 | Public Website -> Salesforce | Data | Remote Call-in | 使用 web-to-lead 捕获线索。线索包含公司信息、办公地址、电话号码和潜在客户的询价等信息。 | HTTPS (单向 SSL/TLS) | 无，建议实施 DOS 机制（例如 CAPTCHA） |
| Int-002 | IoT sensors -> Heroku | Data | Remote Call-in | 传感器大约每 30 秒向 Heroku 发送定期状态更新。 | HTTPS (单向 SSL/TLS) | OAuth 2.0 资产令牌流 |
| Int-003 | Salesforce <-> Heroku | Data | Batch data sync – async | 使用 Heroku Connect 在 Salesforce Core Cloud (Force.com) 和 Heroku 之间进行双向数据同步。假设对象：账户和案例。 | HTTPS (单向 SSL/TLS) | OAuth 2.0 Web 服务器流 |
| Int-004 | Tablet app -> MS SharePoint Online | Data | Remote process invocation – request and reply | 使用 SharePoint API 从平板应用上传文档到 SharePoint。 | HTTPS (单向 SSL/TLS) | OpenID Connect 用户代理流 |
| Int-005 | Tablet app -> Salesforce | Data | Remote Call-in | 从 Salesforce 拉取数据，例如客户信息和 ID。 | HTTPS (单向 SSL/TLS) | OpenID Connect 用户代理流 |
| Int-006 | MS SharePoint Online <> Salesforce | Data | Batch data sync – async | 使用 Files Connect，检索存储在 SharePoint 中的文档链接并在 Salesforce 中显示。 | HTTPS (单向 SSL/TLS) | OpenID Connect Web 服务器流 |
| Int-007 | Salesforce <> MuleSoft <> ERPs | Data | Batch data sync – async | 一项计划批处理作业，用于将客户和订单数据从 Salesforce 同步到 ERPs。这些记录的更新应从 ERPs 同步回 Salesforce。计划作业的频率取决于变更量，假设为 15 分钟。 | Salesforce 和 MuleSoft 之间 HTTPS (双向 SSL/TLS)。MuleSoft 与 ERP 之间 HTTPS (单向 SSL/TLS)。 | OpenID Connect JWT 流 |
| Int-008 | Mobile app -> Salesforce | Data | Remote process invocation – request and reply | 使用 Salesforce REST API 检索和更新客户数据，例如个人资料。 | HTTPS (单向 SSL/TLS) | OpenID Connect 用户代理流 |
| Int-009 | Salesforce <> Marketing Cloud | Data | Batch data sync – async | 使用 Marketing Cloud Connector 将联系人和线索等数据从 Salesforce 同步到 Marketing Cloud，反之亦然。 | HTTPS (单向 SSL/TLS) | OAuth 2.0 客户端凭据流（用于向 Marketing Cloud 进行身份验证）OAuth 2.0 Web 服务器流（用于向 Salesforce 进行身份验证） |
| Int-010 | Heroku -> Tableau CRM | Data | Batch data sync – async | 将来自 Heroku 的聚合传感器读数数据复制到 Tableau CRM。 | HTTPS (单向 SSL/TLS) | OpenID Connect Web 服务器流 |
| SSO-001 | SSO for MS SharePoint using Azure AD as IDP | SSO | SSO – SAML 2.0 | 使用 SAML 2.0 进行 SSO，Azure AD 作为 IdP，SP 发起的流。 | HTTPS (单向 SSL/TLS) | SAML 2.0 SP 发起的流 |
| SSO-002 | SSO for Salesforce using Azure AD as IdP | SSO | SSO – SAML 2.0 | 使用 SAML 2.0 进行 SSO，Azure AD 作为 IdP，SP 发起的流。 | HTTPS (单向 SSL/TLS) | SAML 2.0 SP 发起的流 |
| SSO-003 | SSO for ERP systems using Azure AD as IdP | SSO | SSO – SAML 2.0 | 使用 SAML 2.0 进行 SSO，Azure AD 作为 IdP，SP 发起的流。 | HTTPS (单向 SSL/TLS) | SAML 2.0 SP 发起的流 |

## 图 5.7 – 集成接口（最终）

这结束了第一个小型场景。你可以根据需要多次尝试重新解决此场景。建议第一次尝试不设时间限制进行解决，目标是创建尽可能出色的成果物和演示宣传。

然后，你可以尝试添加一些时间限制，例如 90 分钟限制，以创建端到端解决方案。

## 摘要

在本章中，你深入了解了系统架构领域的细节。你了解了对 CTA 的期望以及详细程度。然后，你处理了一个专注于系统架构的小型假设场景，并创建了一些引人注目的演示宣传。

你做出了多个设计决策，同时做出了一些假设和大量理由。在实际项目中，你需要记录所有这些设计决策，以避免未来丢失任何有价值的信息。

然后，你遇到了确定正确组织策略的挑战，并根据共享需求提供了明确且有理由的建议。你还解决了制定移动策略的挑战。你提供了一份所需许可证的清晰列表，并解释了为什么认为它们是每组用户最合适的。

最后，你解决了其他一些挑战，例如推荐 DMS 解决方案。你必须处理棘手的部分：确定是需要平台内还是平台外解决方案来处理特定需求，例如大规模 IoT 数据消耗。

你现在准备进入下一章，你将在其中学习需要掌握的第二个领域，即安全性。

## 章节复习闪卡

在进入下一章之前，建议你先过一遍本章的闪卡。这些闪卡将本章所有概念浓缩成更小、更易管理的小块，有助于快速复习和记忆。通过参与这些闪卡，你将加强对关键主题的理解，识别需要进一步学习的领域，并在进入新概念之前建立信心。

以下图片显示了闪卡界面的示例。

仪表板 > 第 5 章闪卡

第 5 章闪卡

专注模式

点击闪卡上的任意位置以揭示答案。

目前已记忆的闪卡：0
尚未记忆的闪卡：3
标记为已记忆

定义现场服务管理解决方案。

上一个 下一个
2/3

## 图 5.8 – CTA 闪卡界面

要访问本章的章末闪卡，请执行以下步骤：

- 1. 打开网络浏览器并访问 https://packt.link/ctach5。你将看到以下屏幕：

![](img/a3b1eecf3c87c814267f2877e8a8be1a_202_0.png)

## 图 5.9 – 闪卡界面登录

你也可以扫描以下二维码访问网站：

![](img/a3b1eecf3c87c814267f2877e8a8be1a_202_1.png)

## 图 5.10 – 访问第 5 章闪卡的二维码

- 2. 使用你的凭据登录帐户。如果你尚未激活帐户，请参考前言中的《解锁在线内容说明》以获取详细说明。

成功登录后，你将看到以下屏幕：

![](img/a3b1eecf3c87c814267f2877e8a8be1a_203_0.png)

## 图 5.11 – 章节摘要和章末闪卡

- 3. 点击闪卡堆栈上的“开始”以开始复习。

## 在 Salesforce 中制定安全架构

### 你的唯一注册代码

用于解锁在线内容的你的唯一注册代码是 c147tx。注册链接是 https://packt.link/ctasignup。

打开链接，输入代码，并按照前言《解锁在线内容说明》部分中详细说明的步骤完成注册流程。

本章将继续探讨通过 CTA 评审委员会所需的 Salesforce 特定知识领域。这一次，你将处理安全领域。

在当今，随着持续向以客户为中心的解决方案转移以及联网设备的兴起，设计安全系统从未像现在这样重要。

Salesforce 重塑了人们对 CRM 的普遍理解。多家企业正在将 Salesforce 作为其中央参与平台，拥有数十个与不同系统的入站和出站集成。

作为 Salesforce 安全架构师，你需要利用 Salesforce 的丰富功能来设计稳健的安全架构。你还需要定义在数据从其他系统传输到 Salesforce 及从 Salesforce 传输到其他系统过程中保护数据所需的安全措施。你必须掌握许可证类型和对象关系影响数据可见性的不同方式——尤其是在处理 Salesforce 社区或复杂的共享和可见性要求时。

安全是 Salesforce 中最广泛的领域之一。它涵盖许多不同且多样的功能，并且也受其他领域的影响。考虑到它与其他领域（例如数据架构）的紧密关系，你将在后续章节中遇到与安全架构相关的主题。在本章中，你将涵盖以下主要主题：

- 了解作为安全架构师你应该能做什么
- 介绍安全架构领域（小型假设场景）——Packt 创新零售商 (PIR)

