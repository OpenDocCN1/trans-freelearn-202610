

# 本书适合谁

- 希望加深对常见陷阱及其规避方法的理解的 Kubernetes 实践者
- 负责管理和扩展容器化应用的 IT 专业人员
- 希望在 Kubernetes 部署中实施最佳实践的开发人员和系统架构师

# 本书涵盖内容

*第 1 章*，*Kubernetes 反模式简介*，通过定义 Kubernetes 反模式、其重要性以及对运维的影响来奠定基础，引导读者掌握理解 Kubernetes 复杂性所需的初步认知。

*第 2 章*，*识别常见的 Kubernetes 反模式*，深入探讨常见反模式的识别，为读者提供在真实 Kubernetes 环境中发现并理解这些模式影响的工具。

*第 3 章*，*原因与后果*，探讨常见反模式的根本原因及其对 Kubernetes 生态系统的更广泛影响，使读者具备从源头解决这些问题的知识。

*第 4 章*，*实用解决方案与最佳实践*，提供可操作的策略和既定最佳实践，以有效应对和缓解 Kubernetes 反模式，确保部署优化且具有弹性。

*第 5 章*，*真实案例研究*，呈现在真实场景中成功识别并解决 Kubernetes 反模式的深入分析，展示最佳实践和解决方案在不同环境中的应用。

*第 6 章*，*性能优化技术*，聚焦于提升 Kubernetes 部署性能、效率和可扩展性的技术，提供在各种运维环境中最大化 Kubernetes 潜力的见解。

*第 7 章*，*拥抱 Kubernetes 中的持续改进*，讨论对 Kubernetes 部署和管理采取迭代、持续改进方法的重要性，强调适应不断发展的 Kubernetes 生态系统以实现持续运维卓越的策略。

*第 8 章*，*主动评估与预防*，强调培养主动思维以预测和缓解 Kubernetes 环境中的潜在陷阱，详细说明评估策略和预防措施。

*第 9 章*，*整合全书内容*，通过综合各章的关键见解、策略和经验教训来总结全书，指导读者如何应用这些知识来培育稳定、高效且安全的 Kubernetes 环境，并倡导持续改进的文化。

# 如何充分利用本书

您需要对 Kubernetes 环境的基础、容器编排以及 DevOps 实践原则有一定了解，才能有效利用本书的见解。

| 书中涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Kubernetes | Windows、macOS 或 Linux |
| Docker | Windows、macOS 或 Linux |
| 云服务提供商（AWS、Azure 和 Google Cloud） |  |
| 持续集成工具 | Windows、macOS 或 Linux |
| 监控工具（Prometheus、Grafana 等） | Windows、macOS 或 Linux |

# 使用的约定

本书中使用了多种文本约定。

**正文中的代码**：表示正文中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、示例 URL、用户输入和 Twitter 账号。示例：“将下载的 **WebStorm-10*.dmg** 磁盘映像文件挂载为系统中的另一个磁盘。

代码块如下所示：

```
Dockerfile
# Stage 1: Build the application
FROM node:16 as builder
```

任何命令行输入或输出均如下所示：

```
kubectl apply -f nginx-ingress.yaml
```

**粗体**：表示新术语、重要词语或屏幕上显示的词语。例如，菜单或对话框中的词语以 **粗体** 显示。示例：“从 **Administration** 面板中选择 **System info**。”

*TIPS OR IMPORTANT NOTES*

*以此形式出现。*

# 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 [customercare@packtpub.com](customercare@packtpub.com)，并在邮件主题中提及书名。

**勘误**：尽管我们已尽一切努力确保内容的准确性，但难免会出错。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 [www.packtpub.com/support/errata](www.packtpub.com/support/errata) 并填写表格。

**盗版**：如果您在互联网上发现我们作品的任何形式的非法拷贝，我们将不胜感激您提供位置地址或网站名称。请通过 [copyright@packt.com](copyright@packt.com) 联系我们并附上材料链接。

**如果您有兴趣成为作者**：如果您在某个领域有专业知识并有兴趣撰写或为书籍做出贡献，请访问 [authors.packtpub.com](authors.packtpub.com)。

# 分享您的想法

在您阅读完 *Kubernetes Anti-Patterns* 后，我们很想听听您的想法！请 [click here to go straight to the Amazon review page](click here to go straight to the Amazon review page) 为本书留下反馈。

您的评论对我们和技术社区都很重要，有助于确保我们提供优质内容。

# 下载本书的免费 PDF 副本

感谢您购买本书！

您喜欢随时阅读但无法随身携带印刷书籍吗？

您的电子书购买与您选择的设备不兼容？

不用担心，现在每本 Packt 书籍都免费提供一份无 DRM 的 PDF 版本。

在任何地方、任何地点、在任何设备上阅读。直接从您喜爱的 technical 书籍中搜索、复制和粘贴代码到您的应用程序中。

福利还不止于此，您可以每天通过电子邮件获得独家折扣、新闻通讯和精彩免费内容

按照以下简单步骤获取福利：

- 1. 扫描二维码或访问下方链接

![](img/ffb66b2e42c0578848d97de5502b7960_38_0.png)

https://packt.link/free-ebook/9781835460689

- 2. 提交您的购买证明
- 3. 就这样！我们会直接将免费 PDF 和其他福利发送到您的电子邮件

# 第 1 部分：理解 Kubernetes 反模式

在本部分，您将全面了解 Kubernetes 反模式，包括其起源、影响以及如何在实际场景中识别它们。本部分涵盖识别和解决常见反模式以有效优化 Kubernetes 部署的基本策略。

本部分包含以下章节：

- *第 1 章*，Introduction to Kubernetes Anti-Patterns
- *第 2 章*，Recognizing Common Kubernetes Anti-Patterns
- *第 3 章*，Causes and Consequences

# Kubernetes 反模式简介

在容器编排和云原生技术不断发展的格局中，Kubernetes 成为一座闪亮的灯塔，为管理容器化应用提供了无与伦比的强大功能和灵活性。然而，这种强大功能是有代价的——Kubernetes 的复杂性有时会导致即使是最有经验的从业者也走上危险的道路，导致部署不佳、运维难题，最终损害应用性能。

Kubernetes，通常被称为 K8s，是一个由相互连接的组件构成的复杂生态系统，每个组件都有其自己的最佳实践、配置和潜在陷阱。虽然这种复杂性使组织能够构建高度弹性和可扩展的应用，但也为失误创造了机会。Kubernetes 环境并不免受捷径、临时修复或错误配置的诱惑，导致的问题属于反模式范畴。

在深入探讨 Kubernetes 反模式的具体内容之前，了解前方的路线图至关重要。后续章节将带您了解一系列反模式，阐明它们带来的挑战并提供规避指导。

