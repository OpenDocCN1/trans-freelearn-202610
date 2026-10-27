

# 11

# Microsoft Sentinel 中的威胁狩猎

# 第 4 部分：集成与自动化

# 12

# 创建 Playbook 和自动化

# 前言

Microsoft Sentinel 是微软开发的智能安全服务，专注于集成云安全和人工智能。*Microsoft Sentinel in Action* 将帮助你获得足够的理解，以充分利用 Azure 服务来抵御现代网络安全威胁。

在 Ignite 2021 期间，微软宣布 Azure Sentinel 将更名为 Microsoft Sentinel。然而，在所有地方更改名称是一项非常耗时的任务，并且在本书完成时，该过程尚未完成。因此，部分图片中仍显示 Azure Sentinel 而非 Microsoft Sentinel，因为名称更改在 Azure 门户中尚未完成，但功能保持不变。

## 本书适合谁

如果你是一名 IT 专业人员，之前有使用其他微软安全产品和 Azure 的经验，现在希望扩展知识以纳入 Microsoft Sentinel，那么本书适合你。使用替代 SIEM 工具的安全专家希望将 Microsoft Sentinel 作为附加服务或替代方案采用，也会觉得本书有用。

## 本书涵盖内容

*第 1 章，开始使用 Microsoft Sentinel*，包括了我们今天所看到的云安全架构概述。这些信息为理解现代 **安全运营中心 (SOC)** 是什么以及 Microsoft Sentinel 在其中发挥的关键作用奠定了基础。我们将重点关注创建 SOC 平台所需的特定组件，以及 Microsoft Sentinel 如何将所有数据汇集在一起以提供集中分析和行动能力。

*第 2 章，Azure Monitor – Log Analytics 简介*，重点介绍 Azure Log Analytics 工作区的创建，这是我们存储所有日志数据供 Microsoft Sentinel 分析的地方。这是配置 Microsoft Sentinel 的重要第一步。

第 15 章，持续学习和社区贡献，将通过提供获取最新信息的位置指导以及如何为正在发展以支持安全相关信息和技术开发与共享的社区做出贡献来结束本书。

## 如何充分利用本书

我们建议你能够访问一个 Azure 环境，在该环境中你拥有创建 Microsoft Sentinel 环境的适当权限。之前使用过 Azure 门户也会有所帮助。

| 书中涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| 活跃的 Azure 订阅 | Windows、Mac OS 或 Linux（任意） |

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。你可以在此处下载：https://static.packt-cdn.com/downloads/9781801815536_ColorImages.pdf。

## 使用的约定

本书中使用了多种文本约定。

文本中的代码：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。以下是一个示例："在以下示例中，当查看 state 为 NORTH CAROLINA 的行时，除了 State 和 duration 之外的所有列都将为空，因为 NCEvents 表只有 State 和 duration 列。"

代码块如下所示：

```
let FLEvents = StormEvents
| where State == "FLORIDA";
let NCEvents = StormEvents
| where State == "NORTH CAROLINA"
| project State, duration = EndTime - StartTime;
NCEvents | union FLEvents
```

**粗体：** 表示新术语、重要词语或你在屏幕上看到的词语。例如，菜单或对话框中的词语在文本中以这种方式出现。以下是一个示例："要运行本章的示例，你需要展开屏幕左侧的 **Samples** 日志，然后选择 **StormEvents**。"

任何命令行输入或输出均写为如下：

```
StormEvents
| distinct State
| order by State asc
```

提示或重要说明
显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果你对本书的任何方面有疑问，请在邮件主题中提及书名并发送邮件至 `customercare@packtpub.com`。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果你发现本书中的错误，我们将非常感激你向我们报告。请访问 www.packtpub.com/support/errata，选择你的书籍，点击勘误提交表单链接并输入详细信息。

**盗版：** 如果你在互联网上发现我们作品的任何形式的非法副本，我们将非常感激你提供位置地址或网站名称。请发送邮件至 `copyright@packt.com` 并附上材料链接。

**如果你有兴趣成为作者：** 如果你有专业知识的主题并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。

## 第 1 部分：
设计和
实施

在本节中，你将对 Microsoft Sentinel 有一个概述，包括当前的云环境、云安全参考框架、安全运营中心 (SOC) 平台组件以及如何映射架构。你还将了解 Azure Monitor Log Analytics 资源，包括规划你的 Log Analytics 实例、如何创建新实例以及将其附加到 Microsoft Sentinel。

本节包含以下章节：

- 第 1 章，开始使用 Microsoft Sentinel
- 第 2 章，Azure Monitor – Log Analytics 简介

- Microsoft Sentinel 的服务定价
- 场景映射

## 当前的云安全环境

要了解你的安全架构需求，你必须首先确保对你试图保护的 IT 环境有扎实的理解。在部署任何新的安全解决方案之前，需要绘制出当前部署的解决方案以及它们如何保护 IT 环境的各个区域。以下列表提供了任何现代 IT 环境的主要组件：

- 不利于安全工作的终端用户习惯
- 用于身份验证和授权系统访问的身份
- 用于访问内部资源和互联网的网络
- 用于内部应用程序和敏感信息的数据中心中的存储和计算
- 终端用户设备及其用于与数据交互的应用程序
- 在某些环境中，你还可以包括工业控制系统 (ICS) 和物联网 (IoT)

当我们开始查看这些组件的威胁和漏洞时，我们很快就会发现自己深陷于问题和解决方案的字母汤中。

## 网络安全的字母汤

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_22_0.png)

7. **物联网和工业控制系统：** ICS 通常在与企业环境隔离的情况下运行和维护，被称为 **信息技术/运营技术 (IT/OT)** 分隔。这些是高度定制的系统，可能存在数十年且不易更新或更换。网络和设备可能对任何延迟或扫描尝试高度敏感；推荐的方法是被动监控网络流量。

对物联网的引用是不同的，但相似；在这些系统中，将会有大量小型设备收集数据并控制关键业务功能，而无需在同一网络上工作。其中一些设备可以智能化以实现自动化；其他则是单用途的（振动或温度传感器）。从这些系统收集的数据量和速度可能非常高。如果可以从数据中获得有用信息，则考虑在将其摄取到 Microsoft Sentinel 进行分析和短期或长期保留之前先过滤信息。

8. **私有云基础设施：** 这可能托管在本地服务器机房、专门设计的数据中心或第三方提供商处。该组件涉及的技术包括存储、网络、内部防火墙以及物理和虚拟服务器。数据中心是许多公司过去 2-3 十年的支柱，但大多数现在正在转变为混合解决方案，结合云（公共）和本地（私有）解决方案的优点。这里的关键考虑是你可以收集多少日志数据并传输到云中以供 Microsoft Sentinel 摄取。我们将在 *第 3 章，管理和收集数据* 中更详细地介绍数据连接器。

Active Directory 是应包含在此组件中的关键解决方案。它将被扩展到公共云基础设施（组件 09）并在 *特权访问管理* 部分（组件 10）中进行处理。保护 Azure Active Directory 的最佳防御是部署 Microsoft Defender for Identity 解决方案，微软专门为保护 Active Directory 域控制器而开发。

9. **公共云基础设施：** 这些解决方案现在是大多数现代 IT 环境的主流，最初可能是现有本地虚拟化服务器工作负载的扩展、灾难恢复解决方案或由开发人员创建和维护的隔离环境。成熟的公共云部署将在创建和运营的整个生命周期中嵌入多层治理和安全。此组件可能包括 **基础设施即服务 (IaaS)、平台即服务 (PaaS) 和软件即服务 (SaaS)** 服务；每个公共云服务提供商都提供可集成到 Microsoft Sentinel 的自身安全保护。

