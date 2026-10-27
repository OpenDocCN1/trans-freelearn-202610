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
