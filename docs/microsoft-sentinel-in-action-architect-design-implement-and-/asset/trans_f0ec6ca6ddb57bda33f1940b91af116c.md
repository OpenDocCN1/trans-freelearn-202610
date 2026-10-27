

## 理解 STIX 和 TAXII

MITRE 公司是一家非营利性公司，提供框架和标准形式的指导，以协助开发更强大的网络安全控制措施；STIX 语言和 TAXII 协议就是这一开发工作的一些例子。

这两个标准由一个开放的社区共同开发，由美国国土安全部（DHS）赞助，与 MITRE 公司合作。这些不是软件产品，而是产品可以使用的标准，以实现在与您的安全社区和业务合作伙伴共享 TI 信息时实现自动化和兼容性。

根据 MITRE 提供的描述：*STIX 是一个协作的社区驱动的努力，旨在定义和开发一种标准化的语言来表示结构化的网络威胁信息。* STIX 语言的开发是为了确保威胁信息能够以一致的方式共享、存储和使用，以促进自动化和人工辅助分析。您可以在此处阅读有关 STIX 标准的更多信息：https://stixproject.github.io/。

TAXII 协议的开发是为了支持来自开源情报（OSINT）和 TIP 的 TI 源，这些源支持该标准协议和 STIX 数据格式。您可以在此处阅读有关 TAXII 协议的更多详细信息：https://www.mitre.org/sites/default/files/publications/taxii.pdf。

Microsoft 已开发了一个连接器，以实现与使用 TAXII 协议的服务的集成，从而能够摄取 STIX 2.0 威胁指标供 Microsoft Sentinel 使用。

> **公开预览**
> 
> 由于敏捷开发的性质和常青云环境，Microsoft 首先通过私人预览向少数精选评审员提供新功能，然后发布到公开预览，以允许更广泛的受众参与反馈。您的组织需要确定是否可以接受在生产租户中使用预览功能，或将其访问限制为仅开发/测试环境。截至撰写本文时，TAXII 数据连接器处于公开预览中。您可以在 Microsoft Sentinel 租户中查看可用数据连接器的列表，以查看它是否当前可用。

在下一章中，我们将回顾情报源的选项，并选择最适合您组织需求的选项。

## 为您的需求选择合适的情报源

借助 Microsoft Sentinel，您可以从多个来源导入 TI，以提高安全分析师识别和优先处理已知威胁和 IoC 的能力。配置后，以下 Microsoft Sentinel 工具中将提供多个可选功能：

- **分析**：提供包含 ThreatIntelligenceIndicator 字段的能力，以确保分析规则可以将日志事件与已识别的域、电子邮件帐户、文件哈希、IP地址或 URL 匹配。
- **工作簿**：创建了一个新的工作簿，用于显示摄取到 Microsoft Sentinel 中的 TI 的数量、类型和置信度，以及有多少警报是由匹配威胁指标的分析规则生成的。
- **狩猎**：与分析规则类似，启用 TI 为狩猎查询提供将日志事件与 TI 信息进行比较的能力，使安全调查人员在狩猎场景中具有更大的能力。
- **笔记本**：笔记本可用于集成 TI 的指导狩猎，这使分析师和安全调查人员能够寻找异常并搜寻恶意行为。

有几种可用的选项可以获取 TI 源，您也可以根据通过内部 IT 调查收集的特定信息生成自己的指标；这使您可以开发一个已知特定于您组织的指标的独特列表。根据您的行业和地区，您可以选择与合作伙伴组织和社区共享这些信息，以获取与医疗保健、政府、能源等相关的特定信息。

您可以选择利用与 **Microsoft Graph Security tiIndicator** API 的直接集成，其中包含 Microsoft 自己的 TI，收集自其庞大的互联网服务环境（例如 Azure、Office 365、Xbox 和 Outlook）。

为了实现最广泛的情报范围，建议您还从开源平台获取 TI 源，例如以下内容：

- MISP 开源 TIP
- Palo Alto Networks 的 MineMeld
- 任何基于 MITRE STIX/TAXII 标准的内容

- 概述
- 开始使用
- 预览功能
- 诊断和解决问题

## 管理

- 用户
- 组
- 外部身份
- 角色和管理员
- 管理单元
- 企业应用程序
- 设备
- 应用注册
- 身份治理
- 应用程序代理
- 许可证

图 4.3 – Azure AD 应用注册

2. 在工具栏中，选择 + New registration，如以下屏幕截图所示：

- + New registration
- Endpoints
- Troubleshooting
- Download
- Preview features

图 4.4 – 创建新的应用注册

当您配置集成的 TIP 产品时，您将需要参考这些 ID 值。

5. 接下来，我们将为我们注册的应用程序配置 API 权限。为此，请转到左侧菜单并选择 **API permissions**，如以下屏幕截图所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_119_0.png)

图 4.7 – API 权限菜单选项

6. 创建时，将有默认权限 **User.Read**。选择 **+ Add a permission**。

现在应用已在 Azure AD 中注册，我们可以在 TI 服务器/服务的注册过程中使用它来运行并向 Microsoft Sentinel 发送信息。

接下来，我们可以开始配置 MineMeld 服务器，这涉及设置新的 **虚拟机（VM）** 来收集和转发 TI 源。

## 配置 MineMeld TI 源

完成之前的步骤后，您就可以配置将 TI 数据发送到 Microsoft Sentinel 的服务。本节将指导您完成 MineMeld TI 共享解决方案的配置。

> **注意**
> 如果 MineMeld TI 服务器尚未在您的环境中运行，您将需要配置该产品；此指导仅适用于 Microsoft Sentinel 连接器。

此过程包含三个步骤：

- 1. 构建 VM 并安装 MineMeld
- 2. 在 MineMeld 中安装 Microsoft Graph Security API 扩展
- 3. 配置扩展以通过 Security API 连接到 Microsoft Graph

要执行此操作，您需要对 Azure 租户和 MineMeld 服务具有管理员访问权限。

## 构建 VM 并安装 MineMeld

> **注意**
> 在测试时，有必要确保仅使用 Ubuntu 版本 16.04；其他版本可能不兼容。

理想情况下，您已在环境中配置并运行 MineMeld VM；但是，如果没有，您可以通过 Palo Alto Networks 官方网站在线找到配置说明：https://live.paloaltonetworks.com/t5/MineMeld-Articles/Manually-Install-MineMeld-on-Ubuntu-16-04/ta-p/253336。

## 在 MineMeld 中安装 Microsoft Graph Security API 扩展

以下是安装 API 扩展的步骤：

- 1. 登录 MineMeld Web 门户，然后转到 **SYSTEM** 然后 **EXTENSIONS**。
- 2. 单击 Git 图标并输入以下地址：`https://github.com/PaloAltoNetworks/minemeld-msgraph-secapi.git.`
- 3. 单击 **RETRIEVE**，然后从下拉列表中选择 master 并单击 **INSTALL**，如以下屏幕截图所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_123_0.png)

图 4.12 – 从 Git 安装扩展

4. 确认安装已完成，然后选择十字旁边的 **Activate** 按钮。为了确认激活，API 扩展应显示白色背景而不是灰色哈希背景，如以下屏幕截图所示：

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_123_1.png)

图 4.13 – 确认 API 扩展的安装

