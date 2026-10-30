

当我们有一份图纸清单时，项目团队或 BIM 协调员可以快速创建所有图纸，并赋予其正确的名称、编号、状态和其他信息。**这一过程对于提高交付图纸的生产率至关重要。** 我们可以自动化此过程，直接在 Revit 中创建多张图纸，或使用 Dynamo 和 Revit 插件来加快此过程。

关键点在于明确将向设计团队交付的成果物，并为图纸制作分配团队成员的职责。

一旦图纸已创建，项目团队或 BIM 协调员就可以使用 Revit 视图填充大部分图纸，即使这些视图尚未完成或非常基础。随着项目的推进，图纸将开始成形，项目经理可以打印图纸集以验证进度。

这套初始图纸称为 **Cartoon Set**，它有助于规划项目交付物。Cartoon Set 提高了效率，因为我们可以确保图纸集包含交付所需的所有必要信息。

随着项目的推进，Cartoon Set 将成为一套完整的图纸，这有助于我们顺利高效地满足截止日期。

> **Tip**
> 在截止日期前最后一刻创建多张图纸是个坏主意！

## Work with a Good Revit Template

图纸的生产效率将取决于 Revit 模板的质量，该模板将包括线宽、视图模板、标题图纸和其他 Revit 设置。

在 *Chapter 3, Revit Templates and Standards* 中，我们学习了如何创建和改进 Revit 模板。

模板中包含的行业和组织标准将促进项目之间的统一，并使其与图形和技术标准保持一致。

## Control Drawing Visibility

我们可以通过多种方式控制图纸可见性，例如使用 **View Templates** 和其他技术，但一些流程可以帮助提高生产率，以便我们可以以 PDF 和 DWG 格式创建图纸。

- 阴影会增加创建 PDF 文件所需的时间。如果不需要，请关闭阴影。
- 检查链接文件是否对特定视图是必要的。如果我们卸载不必要的链接，Revit 的性能将提高。
- 确保已安装最新的打印机驱动程序或插件以打印 PDF 文件。在 *Chapter 9, Enhancing Productivity with Plugins* 中，我们将了解更多关于 Revit 插件的信息。

在本节中，我们了解了在 Revit 中制作图纸时可能影响生产率的三个因素。

## Summary

在本章中，我们学习了 Revit 的核心功能和最佳实践，以提高使用施工规范的明细表和注释的工作效率。明细表工作流程分为基础和高级，以帮助涵盖管理明细表的必要流程。

在 Revit 中，注释用于将施工规范分配给 Revit 组件，从而在注释图纸时提高生产率和一致性。

我们随后解释了设计选项、阶段和范围框的最佳实践和工作流程，这有助于我们在使用 Revit 进行日常活动时提高效率。我们还学习了工作共享的工作流程，以便我们可以与多个同时在同一文件中工作的用户协作。

之后，我们介绍了在图纸制作期间提高生产率的三个主要因素，包括定义图纸清单、使用良好的 Revit 模板以及控制图纸的可见性。

在下一章中，我们将介绍 Dynamo 的视觉编程，并学习如何自动化 Revit 中的特定任务。

## Section 3:
Adding
Visualizations and
Dashboards to
Your Data

本节包含以下章节：

- *Chapter 6, Visual Programming with Dynamo*
- *Chapter 7, Dashboard Analyses with Power BI*

在这些章节中，您将获得使用 Dynamo 提高 Revit 生产率所需的基本知识，并学习如何使用 Revit 数据在 Power BI 中运行多次分析。

以下主题将被涵盖：

- Dynamo BIM 的基础知识
- 精通 Dynamo 以提高 Revit 效率的重要性
- 如何从 Revit 导出和导入数据到 Power BI
- 使用 Power BI 分析 Revit 数据

## 6
Visual Programming with Dynamo

视觉编程使用户能够通过更改图形元素而不是文本编程来创建脚本，而文本编程是开发代码的典型过程。基本上，我们是在不编写代码的情况下创建算法：

![](img/1961add582dd4a589b2eac3dda140e38_315_0.png)

Figure 6.1 – Dynamo graphical elements

用户无需任何编程经验即可开始使用 Dynamo 等视觉编程应用程序并自动化 Revit 任务，但这需要投入学习和掌握新流程。

Dynamo 是一款开源软件，可为 Revit、Civil 3D 和其他软件提供视觉编程。

Dynamo 的原理遵循三个基本流程：

**INPUT + FUNCTIONS AND OPERATIONS + OUTPUT**

我们将能够通过使用 Dynamo 自动化多个工作流程并快速完成重复性任务，从而提高 Revit 中的生产率。

本章将为您提供 Dynamo 的基本知识，并展示掌握该应用程序以提高 Revit 效率的重要性。

本章还将为您介绍 Dynamo，展示最常用的 Dynamo 包，并解释如何使用 Dynamo 进行某些工作流程。

本章将涵盖以下主题：

- Dynamo
- Expanding Dynamo
- Best practices

## Technical requirements

在本章中，您需要访问 Autodesk Revit。用于描述所有这些练习的版本是 Revit 2021，但您可以使用其他版本进行跟进。需要注意以下几点：

- 不同版本之间可能略有不同。
- Revit 的旧版本无法打开较早版本的 文件。

本章使用的所有数据集都可以在本书的 GitHub 仓库中找到，地址为
`https://github.com/PacktPublishing/-Increasing-Autodesk-Revit-Productivity-for-BIM-Projects`。

## Introducing Dynamo

业务中的生产率是公司绩效的重要因素。提高生产率是在预定义的时间范围内更智能地工作以满足项目截止日期的过程。

生产率方法如下：

- 时间管理
- 劳动力完成的任务
- 个人绩效
- 制造产品的比率

Dynamo 将通过提高您完成重复性任务的绩效以及处理复杂几何的能力来提高生产率。

Dynamo 是一个开源应用程序，可以作为独立的 *Sandbox* 使用，也可以与 Revit、Maya、Civil3D 或任何其他具有 **API**（**Application Programming Interface** 的缩写）可用的应用程序连接。

它旨在通过自动化流程和工作流程来扩展 Revit 的功能。它不会为 Revit 添加新工具，但它将帮助自动化现有工具，以实现几乎无限的任务和建模流程。

让我们开始从 Revit 访问 Dynamo：

1. 打开一个新的 Revit 项目。
2. 从 **Manage** 选项卡，前往 **Visual Programming** 面板。

![](img/1961add582dd4a589b2eac3dda140e38_317_0.png)

Figure 6.2 – Visual Programming panel

3. 点击 **Dynamo.**

点击后，**Dynamo** 窗口将出现：

![](img/1961add582dd4a589b2eac3dda140e38_318_0.png)

Figure 6.3 – Dynamo canvas

有关 Dynamo 的更多信息，请参阅以下链接：

```
https://bit.ly/3b9vxxa
```

QR code:

![](img/1961add582dd4a589b2eac3dda140e38_318_1.png)

