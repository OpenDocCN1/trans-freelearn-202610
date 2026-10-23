## 贡献者

## 关于作者

**Marius Sandbu** 是挪威 Sopra Steria 公司的云布道者和架构师，在 IT 行业拥有超过 17 年的经验。Marius 在身份、网络、虚拟化、端点管理和基础设施等不同技术领域拥有广泛的技术经验，尤其专注于公有云。他是一位热心的博主，共同主持 CloudFirst 播客，并且是 Microsoft Ignite 和 Citrix Synergy 等活动的国际演讲者。他曾在 Tietoevry 工作，担任公有云部门的技术负责人，还曾在奥斯陆大学担任系统管理员。

## 关于审稿人

**Matt Davidsson** 是一名高级系统工程师，在 IT 行业拥有超过 20 年的经验，专注于安全领域。过去几年，他一直担任 CISO，并结合 Microsoft 365 安全生态系统进行实际操作。此前，他是微软认证讲师，并在世界各地举办关于 Microsoft 365、Azure 和其他安全产品的培训课程。

**Nitish Anand** 是一位经验丰富的网络安全专业人士，在该领域拥有 8 年经验。持有**注册信息系统安全专家（CISSP）**认证，他对安全原理和实践有深刻理解，并精通识别和缓解潜在威胁。他目前在微软担任安全分析师。他致力于跟上最新的网络安全趋势和最佳实践，以确保敏感信息和系统的安全。他对行业标准和法规有深刻理解，能够有效沟通并教导他人保护网络和系统的最佳实践。

## 前言

勒索软件攻击可能对组织造成重大损害。它们可能导致敏感或机密数据的丢失、停机、赎金支付造成的财务损失、组织声誉受损以及潜在的法律责任。我们也见过公司在遭受勒索软件攻击后破产的案例。

在本书《*Windows 勒索软件检测与防护*》中，我们将探讨勒索软件攻击日益增长的威胁，以及如何最好地保护自己和组织免受其侵害。你将了解不同类型的勒索软件、攻击者使用的战术和技术，以及在 Windows 设备上检测和预防这些攻击的最佳实践。读完本书后，你将具备保护企业免受勒索软件侵害并保护有价值数据的知识和技能。

本书的写作方式易于理解，适合所有技术水平的读者，从初学者到经验丰富的 IT 专业人士。

## 本书适合谁阅读

IT 管理员、安全管理员、CISO 或其他安全相关角色可以从本书中获得实用见解，了解勒索软件的工作原理以及如何最好地保护其组织免受此类攻击。

## 本书涵盖内容

*Chapter 1, Ransomware Attack Vectors and the Threat Landscape*，解释了勒索软件攻击的工作原理基础、攻击链以及常用的不同攻击向量。该章节还涵盖了一些已知的勒索软件组织以及他们使用过的攻击方法。

*Chapter 2, Building a Secure Foundation*，提供了跨所有不同攻击面不同对策的高层次概述，例如网络、基础设施、终端、身份和 SaaS 服务。它还深入介绍如何构建安全的网络基础和 Windows 环境。

*Chapter 3, Security Monitoring Using Microsoft Sentinel and Defender*，教你如何使用 Microsoft Sentinel 和 Microsoft Defender for Cloud 等服务为基于 Windows 的环境配置和设置安全监控。它还涵盖架构设计、实施以及关于需要监控的关键事件的最佳实践。

*Chapter 4, Ransomware Countermeasures – Windows Endpoints, Identity, and SaaS*，带你了解如何使用 Azure AD 和 Microsoft Endpoint Manager 等功能来保护基于 Windows 的终端的不同对策。此外，它还涵盖了使用密码策略保护终端用户身份的不同方法以及在设备和 SaaS 服务中监控终端用户活动的方法，然后探讨通过电子邮件减少网络钓鱼攻击风险的方法。

_Chapter 5, Ransomware Countermeasures – Microsoft Azure Workloads_，涵盖 Windows Server 和虚拟基础设施其他部分内的不同对策和安全机制。它还涵盖了虚拟基础设施网络分段的一些最佳实践，并包括 Microsoft Azure 内的最佳实践指南。

_Chapter 6, Ransomware Countermeasures – Networking and Zero-Trust Access_，涵盖终端用户连接的网络分段最佳实践以及基于 Windows 终端的安全性，以及如何保护我们的外部 Web 服务免受 DDoS 攻击。它还涵盖 SASE 服务模型以及它们如何帮助降低移动劳动力的风险。

_Chapter 7, Protecting Information Using Azure Information Protection and Data Protection_，详细介绍了加密数据以降低敏感信息落入攻击者手中的风险的不同方法。它还涵盖了 Azure Information Protection 等服务以及与数据保护和备份相关的其他最佳实践。

_Chapter 8, Ransomware Forensics_，解释了如何在受影响的系统上进行取证，以及当企业遭受攻击影响时如何组织工作。它还涵盖了一些寻找攻击者如何入侵系统的证据的不同方法。

_Chapter 9, Monitoring the Threat Landscape_，专注于监控威胁态势的不同提示和工具，以及使用 Microsoft Defender 外部攻击面管理 和 GreyNoise 等工具监控自身安全风险的使用。

_Chapter 10, Best Practices for Protecting Windows from Ransomware Attacks_，包括 Windows 中的最佳实践和安全设置，例如 LAPS、Windows 防火墙 和 Tamper Protection，以及如何保护机器免受 Mimikatz 等凭据收割工具的侵害。最后，它介绍了如何使用更新管理工具使基础设施保持最新。

## 如何充分利用本书

本书的大部分内容使用 Microsoft 的云服务，例如 Microsoft 365 安全服务和 Microsoft Azure 的云服务。为了充分利用本书，你应该能够访问测试环境或使用试用账户搭建自己的环境。

例如，你可以使用试用版访问 Microsoft 365 服务。你可以在此处注册 Azure 试用版：`https://azure.microsoft.com/en-gb/free/active-directory/`。

在注册免费 Azure 试用账户后，你还可以注册 Microsoft E3 或 E5 许可证作为 30 天试用。

此外，你还应能够访问一台可用于管理的示例虚拟机，该虚拟机也可以在 Microsoft Azure 中运行。

需要注意的是，在本书撰写期间，Microsoft 正在更改流程，以便试用账户在试用期结束后自动转换为付费账户，因此请确保随后删除你的许可证或租户。

此外，本书中我们将介绍的大部分内容都基于 Windows。因此，你应很好地了解 Windows 及其核心组件。

**如果你使用的是本书的数字版，我们建议你自己输入代码或从本书的 GitHub 仓库访问代码（链接在下一节提供）。这样做将有助于你避免与复制和粘贴代码相关的任何潜在错误。**

*本书的大部分内容将每周更新，以跟上当前威胁。*

## 下载示例代码文件

你可以从 GitHub 在 https://github.com/PacktPublishing/Windows-Ransomware-Detection-and-Protection 下载本书的示例代码文件。如果代码有更新，将在 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 获取。快来看看吧！

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。你可以在此处下载：https://packt.link/TVueG

## 使用的约定

本书中使用了多种文本约定。

文本中的代码：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。以下是示例：“阻止具有 .exe、.hta、.js 或 .iso 等文件扩展名的文件是一个不错的开始。”

代码块设置如下：

```
"policyRule": {
  "if": {
    "allOf": [
      {
        "field": "location",
        "notIn": "norwayeast
      },
      {
        "field": "location",
        "notEquals": "global"

```

```
},
]
},
"then": {
    "effect": "deny"
```

任何命令行输入或输出均按如下方式书写：

```
$varHours = 72
Get-GPO -All | Where {((Get-Date) - [datetime]$_.ModificationTime).Hours -lt $varHours}
```

**粗体**：表示新术语、重要词语或你在屏幕上看到的词语。例如，菜单或对话框中的词语以**粗体**显示。以下是示例：“在配置门户中，转到 **Customization | Policy Management**，然后单击 **Create** 开始向导。”

**提示或重要说明**
显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果你对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但难免会出错。如果你发现本书中的错误，我们将不胜感激你向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版：** 如果你在互联网上发现我们作品的任何形式的非法副本，我们将不胜感激你提供位置地址或网站名称。请发送邮件至 `copyright@packt.com` 并附上材料链接。

**如果你有兴趣成为作者：** 如果你有专业知识的主题，并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。





## 分享您的想法

在您阅读完 *Windows Ransomware Detection and Protection* 后，我们很乐意听取您的想法！请点击这里直接前往本书在亚马逊的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供优质的内容。
