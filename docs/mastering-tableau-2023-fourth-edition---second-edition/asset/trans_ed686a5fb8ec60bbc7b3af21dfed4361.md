

# 精通 Tableau 2023，第四版

本版最畅销的 Tableau 指南将教您如何在 BI 领域的各种范式中利用 Tableau 的最新功能和产品。本书更新了全新主题，涵盖 Tableau Server、Prep 和 Desktop 的最新功能，以及最新的示例和针对现实挑战的解决方案，将带您从掌握 Tableau 核心概念进阶到高级功能。此外还新增了一个关于数据治理的章节。

通读本书，您将学习如何使用各种文件和数据库连接到 Tableau Desktop 和 Prep Builder，轻松执行数据准备和处理；通过实用示例执行复杂的联接、空间联接、联合和数据混合任务。您还将掌握数据加密的执行，并探索其他专家级示例，帮助您使用 Tableau Exchange 进行计算、制图和可视化设计。

后续章节将通过实操示例教您有关提升仪表板性能、连接 Tableau Server 以及理解数据可视化的所有知识。最后，您将涵盖自助式分析、时间序列分析和地理空间分析等高级用例，并将 Tableau 连接到 Python 和 R，以在 Tableau 中实现编程功能。

最后，新增了一个全新章节，涵盖一个非常热门的主题：数据治理。您将探索 Tableau Server 重要功能的介绍，这些功能将助您做好准备，以符合 GDPR 和其他数据隐私及治理法规。

读完这本 Tableau 书籍后，您将精通 Tableau 2023，并能够应对数据分析领域的常见和高级挑战。

## 适读人群

本书专为业务分析师、商业智能专业人士和数据分析师设计，旨在帮助他们精通 Tableau，以解决一系列数据科学和商业智能问题。如果您有 Tableau 使用经验，将能更快上手相关功能，但这并非先决条件。

## 本书内容涵盖

- *第 1 章：回顾基础知识* — 带您了解开始所需的基本和核心 Tableau 概念。
- *第 2 章：准备您的数据* — 这是一个以理论为导向的章节，将帮助您理解数据准备，从而更轻松地处理混乱数据。
- *第 3 章：使用 Tableau Prep Builder* — 讨论 Tableau Prep Builder，它是 Tableau Desktop 不那么“小”的提取-转换-加载 (ETL) 兄弟产品。
- *第 4 章：了解联接、混合和数据结构* — 解答是使用关系、联接还是混合来组合数据的问题。
- *第 5 章：表计算简介* — 涵盖表计算这一特殊情况，它利用数据顺序分配排名等值。
- *第 6 章：利用 OData、数据加密、大数据和 Google BigQuery* — 教您如何利用大数据解决方案和数据加密的概念。
- *第 7 章：实践详细级别计算* — 帮助您理解这一高级主题，它可以更改特定计算的数据聚合方式。
- *第 8 章：超越基础* — 向您介绍高级可视化概念以及 Tableau Exchange、扩展程序、加速器和连接器。
- *第 9 章：使用地图* — 涵盖 Tableau 内部及部分外部制图功能，使用自定义多边形、热力图和分层地图。
- *第 10 章：使用 Tableau 进行演示* — 展示使用 Tableau 进行演示的技巧和窍门。
- *第 11 章：设计仪表板和可视化最佳实践* — 带您了解不同的格式化技巧和设计规则，以最大化可视化的美感。
- *第 12 章：利用高级分析* — 将通过三个可跟随实操的高级用例（自助式、时间序列和地理空间分析）帮助您将分析技能提升到新高度。
- *第 13 章：提升性能* — 针对性能的各个方面进行探讨，旨在赋能您创建快速加载的仪表板，以获得最佳用户体验。
- *第 14 章：探索 Tableau Server 和 Tableau Cloud* — 涵盖 Tableau Server 的不同产品和功能，包括 Ask Data 和 Data Details。
- *第 15 章：集成编程语言* — 展示如何将 R 和 Python 与 Tableau 集成，以启用（近乎）无限的分析能力。
- *第 16 章：建立数据治理实践* — 向您介绍数据治理这一广泛且重要的主题，以及如何利用 Tableau Server 实现合规。

## 如何充分利用本书

具备 Tableau 基础知识将大有裨益，并且在 14 天免费试用期结束后，您将需要 Tableau 许可证。如果您希望充分利用 *第 15 章：集成编程语言*，具备 R 或 Python 的基础知识也将有所帮助。当然，您也可以跟随步骤边学边练。

其他有用的安装将在书中根据需要提及，均属于 Windows 或 Mac 上普通免费软件安装的范畴。

## 下载示例代码文件

本书的代码包也托管在 GitHub 上：[https://github.com/PacktPublishing/Mastering-Tableau-2023-Fourth-Edition](https://github.com/PacktPublishing/Mastering-Tableau-2023-Fourth-Edition)。我们还有来自丰富图书和视频目录的其他代码包，可在 [https://github.com/PacktPublishing/](https://github.com/PacktPublishing/) 获取。快去看看吧！

## 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书使用的屏幕截图/图表的彩色图片。您可以在此下载：[https://packt.link/TybKH](https://packt.link/TybKH)。

## 约定俗成

全书使用了多种文本约定。

`CodeInText`：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如：“记录性能时，Tableau 最初会在 `My Tableau Repository\Logs` 中创建一个名为 `performance_[timestamp].tab` 的文件。”

代码块设置如下：

```
SCRIPT_REAL("x <- lm(.arg1 ~ .arg2 + .arg3 + .arg4) x$fitted", SUM(Profit), COUNT(Quantity), SUM(Sales), AVG(Discount) 
)
```

**粗体**：表示新术语、重要单词或屏幕上出现的单词（例如菜单或对话框中的单词），在文本中也以此形式显示。例如：“将 **Profit_Expected** 放在 **Columns** 货架上，紧邻 **Profit**。然后点击其中任意一个并启用 **Dual Axis**。”

警告或重要提示显示如下。

提示和技巧显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：发送邮件至 `feedback@packtpub.com`，并在邮件主题中注明书名。如果您对本书的任何方面有疑问，请发送邮件至 `questions@packtpub.com`。

**勘误**：虽然我们已尽一切努力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激如果您能向我们报告。请访问 [http://www.packtpub.com/submit-errata](http://www.packtpub.com/submit-errata)，选择您的书籍，点击 **Submit Errata**，并填写表格。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供该位置地址或网站名称。请通过 `copyright@packtpub.com` 联系我们，并附上相关材料链接。

**如果您有兴趣成为作者**：如果您在某个主题上有专长，并有兴趣撰写或为书籍做贡献，请访问 [http://authors.packtpub.com](http://authors.packtpub.com)。

## 分享您的想法

