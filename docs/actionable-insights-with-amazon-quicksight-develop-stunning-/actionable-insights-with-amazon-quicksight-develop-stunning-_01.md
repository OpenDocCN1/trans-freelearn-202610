# Amazon QuickSight 数据可视化与机器学习驱动洞察实用指南

## 致 Gabriela

感谢她无条件的支持和鼓励。

## 贡献者

## 关于作者

**Manos Samatas** 是一位专注于大数据和分析的解决方案架构师。他拥有多年为包括电信、网络安全、医疗保健和公共部门在内的各个行业开发和设计大数据应用程序的经验。他是分析领域的 AWS **认证主题专家**（SME），并持有 AWS 数据分析专项认证和 AWS 解决方案架构师专业认证。Manos 与他的未婚妻 Gabriela 住在伦敦。在空闲时间，他喜欢旅行、与朋友社交以及照料他的植物。

## 关于审稿人

**Madhavan Sriram** 是 Amazon 的数据科学经理，专注于为 Amazon 的运输业务构建数据密集型产品。他的团队每年处理超过 200 TB 的数据，以构建利用机器学习、大数据和可视化技术实现自动化仓库运营的智能产品。他的研究重点领域在于应用环境下的大数据和机器学习。过去，他曾为包括 IBM、丰田和皇家飞利浦在内的多家大型企业工作，以推进其科学和数据格局。他和妻子居住在卢森堡，喜欢与他们的金毛寻回犬“理查德·帕克”在户外度过时光。

**Raquel Marasigan** 是 CBSPI 的信息安全工程师 II 主管，在那里她分析安全策略的有效性。Raquel 获得 Arellano 大学的政治学文学学士学位，主修政治学。她对技术和安全的兴趣促使她继续攻读数据分析学位。她目前正在亚洲与太平洋大学攻读应用商业分析硕士学位，并完成 2022 学年的毕业设计项目。Raquel 很幸运能与作者、AWS Community Builders 和 AWS 用户组的 Jason Dunn、Mia Heard 等其他专家并肩工作，他们在那里分享自己的见解。感谢 Raquel 的两个儿子在审阅本书过程中的鼓励。

## 前言

采用云原生 **商业智能 (BI)** 工具，例如 Amazon QuickSight，使组织能够大规模地从数据中获取洞察。本书是一本使用 Amazon QuickSight 执行从简单到高级任务的实用指南。

你将首先学习 QuickSight 的基本概念以及如何配置数据源。接下来，你将了解 QuickSight 的主要分析构建功能，以开发可视化效果和仪表板。本书还将演示如何使用参数和屏幕控件开发和共享交互式仪表板。随后将介绍带有 URL 操作的高级筛选选项，然后学习如何设置警报和定时报告。之后，你将探索 QuickSight 中的洞察可视化类型，既使用现有洞察，也通过构建自定义洞察。后续章节将向你展示如何将机器学习洞察（例如预测功能、时间序列数据分析、添加叙述和异常值检测）添加到仪表板中。你还将探索自动化操作的模式，并深入了解允许我们控制设置的 API 操作。最后，你将了解嵌入式仪表板和多租户等高级主题。

读完本书后，你将精通 QuickSight 的 BI 和分析功能，这些功能将帮助你创建具有 ML 功能的 BI 应用程序。

## 本书适用人群

本书适用于希望使用来自 AWS 上的现代数据架构的数据通过 Amazon QuickSight 创建交互式仪表板的 BI 开发人员和数据分析师。本书也适合任何希望通过实用示例深入学习 Amazon QuickSight 的人。你需要熟悉一般的数据可视化概念；但是，不需要事先有 Amazon QuickSight 的使用经验。

## 本书涵盖内容

_第 1 章，介绍 AWS 分析生态系统_，首先介绍 AWS 分析生态系统。然后我们将讨论 Amazon QuickSight 如何融入更广泛的生态系统。我们将更深入地了解现代数据架构及其优势和不同组件。最后，我们将为读者提供一个分步指南，以便在其开发环境中设置此架构，并添加我们将在本书后面使用 Amazon QuickSight 创建可视化效果的演示数据。

_第 2 章，Amazon QuickSight 简介_，介绍 Amazon QuickSight 及其作为云原生 BI 工具的主要优势。我们将解释账户创建阶段的各种选项，包括用户授权选项。最后，我们将为读者提供一个分步指南，以设置 QuickSight 账户并配置所需的权限以连接到 Amazon Redshift。

_第 3 章，使用 Amazon QuickSight 准备数据_，重点介绍如何使用 Amazon QuickSight 创建数据源并使用数据集编辑器。我们将提供分步指南，帮助读者在其环境中设置数据源。最后，我们将探讨更高级的操作，例如连接、行级安全控制和计算字段。

_第 4 章，开发可视化效果和仪表板_，介绍 Amazon QuickSight 的主要分析构建功能。我们将首先探索创作者 UI 并解释不同的可视化类型。在添加某些可视化类型并解释其功能后，我们将介绍仪表板和故事的概念，并解释如何将这些仪表板与其他用户共享。最后，我们将探讨如何设计仪表板并创建自定义主题。

_第 5 章，构建交互式仪表板_，探讨如何使用 Amazon QuickSight 开发交互式仪表板。读者将学习如何在仪表板上添加自定义控件，并使用参数为其 BI 应用程序添加交互性。我们还将探讨带有 URL 操作的点击式高级筛选选项。最后，我们将通过 Web 和移动端 QuickSight 应用程序探索读者的用户体验，并解释如何设置警报和定时报告。

_第 6 章，使用 ML 功能和洞察_，探讨 Amazon QuickSight 中的洞察可视化类型。我们将同时使用 QuickSight 建议的洞察并构建自己的自定义洞察。我们将通过分析时间序列数据来添加预测功能，并添加叙述和异常值检测。最后，我们将更深入地了解如何将 Amazon QuickSight 与使用 Amazon SageMaker 部署的模型进行集成。

第 7 章，理解嵌入式分析，深入探讨嵌入式仪表板。我们将描述嵌入式分析背后的架构和业务驱动因素，并解释权限模型。我们将提供一个分步指南，用于为经过身份验证或未经过身份验证的用户设置嵌入式分析。最后，我们将简要介绍如何为 QuickSight 创作者嵌入 QuickSight 控制台。

第 8 章，理解 QuickSight API，探讨使用 QuickSight API 自动化某些操作的模式。我们将了解如何使用模板 API 创建仪表板和重复使用分析。我们还将探讨自动化监控数据集操作的模式，最后，我们将更深入地了解允许我们控制设置的 API 操作。

第 9 章，管理 QuickSight 权限和使用情况，重点介绍数据权限和管理 Amazon QuickSight 操作。我们将解释从数据授权角度看，它如何与 Lake Formation Redshift 和 Redshift Spectrum 表集成。我们将探讨使用 AWS CloudTrail 进行事件报告，并将研究用于管理 QuickSight 使用情况的常见操作。

第 10 章，Amazon QuickSight 中的多租户，讨论 Amazon QuickSight 中的多租户。为了更好地理解，我们将研究一个简单的动手示例。最后，我们将研究一种结合嵌入式分析和多租户两个概念的架构，并解释其实际用例。

## 如何充分利用本书

你需要熟悉一般的数据可视化概念，但不需要任何先前的 Amazon QuickSight 使用经验。此外，我们期望你对 AWS 云有基本的了解。

如果你使用本书的数字版本，我们建议你亲自输入代码或从本书的 GitHub 仓库访问代码（下一节中提供了链接）。这样做将帮助你避免与复制和粘贴代码相关的任何潜在错误。

请确保在不需要时终止所有正在运行的 AWS 实例，以降低成本。

## 下载示例代码文件

你可以从 GitHub 下载本书的示例代码文件，地址为 `https://github.com/PacktPublishing/Actionable-Insights-with-Amazon-QuickSight`。如果代码有更新，将在 GitHub 仓库中进行更新。

我们还在丰富的图书和视频目录中提供其他代码包，网址为 https://github.com/PacktPublishing/。请查看！

## 下载彩色图片

我们还提供一个 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。你可以在此处下载：http://www.packtpub.com/sites/default/files/downloads/9781801079297_ColorImages.pdf。

## 本书使用的约定

本书中使用了多种文本约定。

文本中的代码：表示文本中的代码字词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。示例：“例如，在 QuickSight 中，DeleteDataSet 操作会删除一个数据集。”

代码块设置如下：

```
$aws quicksight update-user --user-name author-iam --role AUTHOR --custom-permissions-name custom-author --email <your-email> --aws-account-id <account-id> --namespace default --region us-east-1
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目将设置为粗体：

```
{
"Status": 200,
"EmbedUrl": "https://us-east-1.quicksight.aws.amazon.com/... ?code=...&identityprovider=quicksight&isauthcode=true",
"RequestId": "21d2ad96-3c2b-42a4-ae10-8eb28b20892c"
}
```





## 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中注明书名。

**勘误**：尽管我们已竭力确保内容的准确性，但错误难免会发生。如果您在本书中发现了错误，我们将不胜感激如果您能向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供位置地址或网站名称。请通过 `copyright@packt.com` 联系我们，并附上材料链接。

**如果您有兴趣成为作者**：如果您在某个主题上有专长，并有兴趣撰写或参与编写一本书，请访问 `authors.packtpub.com`。

## 分享您的想法

读完《Actionable Insights with Amazon QuickSight》后，我们很乐意听取您的想法！请点击此处直接前往本书的 Amazon 评论页面并分享您的反馈。

您的评论对我们和技术社区非常重要，并将帮助我们确保提供高质量的内容。
