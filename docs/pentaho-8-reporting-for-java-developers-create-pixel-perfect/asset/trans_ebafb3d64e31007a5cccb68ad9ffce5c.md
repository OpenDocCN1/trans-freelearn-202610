
## 介绍 Pentaho Data Integration

作为 Pentaho 产品套件的一部分，Pentaho 提供开源数据集成。凭借直观的、图形化的、拖放式设计环境以及经过验证的可扩展、基于标准的架构，数据集成正日益成为组织在传统专有 ETL 或数据集成工具之外的选择。Pentaho Data Integration 在最初版本中被称为 Kettle，因此在你的调查中，你可能会发现它也被称为 PDI、DI 或 Kettle。

你可能知道 ETL 指的是以下流程：数据抽取是从同构或异构数据源中抽取数据，数据转换是将数据转换为适当的格式或结构以便查询和分析，数据加载是将数据加载到最终目标存储库中。本书不会教授 ETL 的基础知识，但你很容易找到大量专门针对此主题的文献。如果你不了解 ETL 原则和最佳实践，可以将 ETL 视为专门用于将数据从源移动到目标的批处理过程。

正如我们在 Pentaho Business Analytics Platform 中所做的那样，本章中你也将使用 Pentaho Data Integration 的社区版，因为它可下载且对所有人免费提供。当然，本章中的描述仅限于介绍以及与报表管理严格相关的内容。如果感兴趣，有大量资源可供你探索并成为该 ETL 工具的专家。

## 安装 Pentaho Data Integration

Pentaho Data Integration 的安装非常简单，只需执行 *解压并运行* 任务。运行 Pentaho Data Integration 的前提条件很简单；它需要 **Java 运行时环境 (JRE)** 8，已安装在你的环境中。支持基于 Windows 的操作系统和基于 Linux 的操作系统。本章中显示的屏幕截图和预览均使用 Ubuntu 16.04.2 LTS 发行版制作。

在安装应用程序之前，你可以直接从官方 Pentaho 社区网站 (http://community.pentaho.com/) 或 Pentaho SourceForge 网站 (https://sourceforge.net/projects/pentaho/) 下载以 ZIP 文件分发的构件。在这两种情况下，你应下载一个名称类似 `pdi-ce-<version>.zip` 的 ZIP 文件。

一旦 ZIP 文件下载到你的环境中，将其解压到最终路径（在基于 Linux 的操作系统上，建议使用 `/opt` 路径），你将在名为 `data-integration` 的文件夹中找到所有 Pentaho Data Integration。

你可能不会相信，但这就是将 Pentaho Data Integration 安装到你的环境中所需的一切。

Pentaho Data Integration 主要是一个在命令行运行的 Java 应用程序，并嵌入 Pentaho Data Integration 引擎。Pentaho Data Integration 附带了一些有用的工具：Spoon，一个允许你设计转换和作业的图形用户界面；Pan 和 Kitchen，用于运行转换和作业；以及 Carte，一个简单的 Web 服务器，允许你远程执行转换和作业。在本章中，你将在用例示例中看到 Spoon 和 Kitchen。

## Pentaho Data Integration 基础

如前所述，Pentaho Data Integration 是一个具有很少图形工具和界面的引擎。根据这一定义，以下各节描述了一些基本概念，以帮助理解其行为和原理。

## 作业和转换

作业和转换可能是 Pentaho Data Integration（以及 ETL 总体而言，但名称不同）中最重要的两个核心概念。
引用 Pentaho 的常见问题解答：

> "转换是关于从源到目标移动和转换行。作业更侧重于高级流程控制：执行转换、失败时发送邮件、通过 FTP 传输文件等等。另一个关键区别是：转换中的所有步骤并行执行，而作业中的步骤按顺序执行。"

正如你从此常见问题解答中读到的那样，转换基于数据流的概念，即大量数据从通用源迁移到通用目标。一个转换可以管理多个流，所有流均并行执行并由 Pentaho Data Integration 引擎管理。

在下面的图表中，你可以看到使用 Spoon 开发的一个非常基本的转换示例：

![](img/9337ac94537bf7e092e1ea6531dce8fd_669_0.png)

![](img/9337ac94537bf7e092e1ea6531dce8fd_669_1.png)

*谈到数据流，这可能是 ETL 引擎最令人印象深刻的优势之一。它能够根据服务器的有限资源并行管理大量数据。这是开发人员使用传统模式构建的常规应用程序的关键区别之一。*

谈到作业，作业在很大程度上基于任务序列的概念，其中必须声明起点。作业能够将其他作业或转换作为子任务包含的能力，使其成为管理复杂迁移和可重用操作的好方法。

在下面的图表中，你可以看到使用 Spoon 开发的一个非常基本的作业示例：

![](img/9337ac94537bf7e092e1ea6531dce8fd_670_0.png)

如图像所示，一个连接将一个转换步骤或作业条目与另一个连接。数据流的方向在图形视图窗格上用箭头表示。连接可以启用或禁用（例如用于测试目的），并且可以是有条件的（例如在成功或失败的情况下）。

作业和转换都可以定义参数和变量。参数被定义为执行的输入，可用于参数化任务。变量在执行流程中定义，可用于参数化任务，正如参数一样。

## 步骤和条目

步骤和条目分别是转换和作业的原子部分。

一个步骤是转换的一部分。Pentaho Data Integration 中提供超过 140 个步骤。这些步骤根据其主要功能进行分类；例如，输入、输出、脚本等。为了执行特定任务，例如从平面文件读取数据、过滤行以及如前例所示将日志记录到数据库，转换中的每个步骤都以特定方式设计。可以配置步骤以执行你需要的任务。一个步骤可以有多个连接——有些连接两个步骤，而有些仅作为步骤的输入或输出。一个名为 Pentaho Reporting output 的步骤可用于管理 Pentaho 报表。

一个作业条目是作业的一部分。作业条目可以为你提供广泛的功能，从执行转换到从 Web 服务器获取文件。Pentaho Data Integration 中提供超过 100 个条目，它们根据功能分组，与步骤的描述类似。可用的条目中没有直接支持 Pentaho 报表的。

## 数据库连接

正如你在 Pentaho Reporting 中学到的那样，对于 Pentaho Data Integration 也是如此，数据库连接用于从源读取或向目标存储库写入。对于 Pentaho Data Integration，这些数据源主要集中在数据库上，但由于步骤和条目的存在，你有机会访问不同类型的源或目标。

数据库连接可以在作业和转换之间共享，并可重复使用以便更好地管理和维护。支持的数据库列表与你在 Pentaho Reporting 中看到的没有区别。正如你所知，它包括各种不同的厂商、类型和成本。
