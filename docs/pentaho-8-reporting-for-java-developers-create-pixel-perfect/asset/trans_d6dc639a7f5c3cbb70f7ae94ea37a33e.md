
## 测试本地化

在正确设置 Pentaho 报表本地化后，您可能希望在所有可能的语言和国家/地区（用一个词来说，称为 locale）中测试它。为了实现这一目标，Pentaho Report Designer 可以配置为使用一种 locale 而不是另一种。

从 Report Designer 用户界面开始，您可以从上部菜单中访问 Pentaho Report Designer 设置，位于 File | Configuration...。选择后，报表配置将显示如下图所示。

在 `core-module` 设置中，您可以识别出 `.environment.designtime.Locale` 属性，该属性未定义默认值。要设置请求的 locale，请选中该属性并填写右侧的文本字段，该字段将变为可编辑。然后单击 OK 按钮并刷新报表以使配置生效。

![](img/9337ac94537bf7e092e1ea6531dce8fd_491_0.png)

## 报表本地化示例

为了在实践中了解本地化如何工作，让我们开发一个本地化 Pentaho 报表示例，您可以在未来的任务中使用。思路是重用之前的示例，将一些静态标签替换为 `resource-label`，并查看报表在几种不同语言中的外观（我们将使用英语和西班牙语作为示例）。

为此，让我们重用我们在第 4 章 *Creating a Report with Report Designer* 中看到的 `report_02.prpt`。如果您遵循了建议，您应该已将报表保存在开发环境中的 `my_report` 文件夹中。如果没有，您可以从 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库下载，从 `Chapter 04 - Creating a report with Report Designer/my_reports` 路径。

打开报表在 Report Designer 中后，在报表资源管理器 Structure 选项卡中选择 `label: Actuals per position title` 元素（您可以在 Master Report | Report Header 下找到它）。要将标签替换为 `resource-label` 元素，让我们单击上部菜单中的 Format | Morph | resource-label 项。此功能将转换元素的类型，同时保持相同的通用属性。为了完成任务，让我们将 `value` 属性（在通用集合中）设置为 `report.title`。

以完全相同的方式（使用 morph 功能），更改 Master Report | Group: DEPARTMENT | Details Body | Details Header 下的三个标签。对于职位标题的标题，将 `value` 属性设置为 `header.positiontitle`。对于实际值的标题，将 `value` 属性设置为 `header.actual`。对于预算的标题，将 `value` 属性设置为 `header.budget`。

现在报表已正确更新，是时候使用正确的键/值对设置资源包了。要设置资源，请选择 File | Resources...，然后选择 `translations.properties` 并单击 Edit 按钮。完成后，用默认值的属性文件填充（在我们的例子中为英语），描述如下： report.title = Actuals per position title
header.positiontitle = Position title

```
header.actual = Actual
header.budget = Budget
```

完成后单击 OK 按钮。要添加新的翻译文件，请按 Create 按钮并将 Entry Name 字段填写为 `translations_es.properties`。按 OK 按钮后，您将看到使用西班牙语 locale 创建了一个新的属性文件。选择新的属性文件并单击 Edit。完成后，用西班牙语键/值对填充属性文件，描述如下：

```
report.title = Título real por posición
header.positiontitle = Título del puesto
header.actual = Real
header.budget = Presupuesto
```

## 免责声明

![](img/9337ac94537bf7e092e1ea6531dce8fd_493_0.png)

*我希望 Google 为我提供了西班牙语的正确翻译。如果没有，我提前向我的西班牙语朋友道歉。这不是我的母语，但我认为西班牙语会是一个不错的替代语言，考虑到它被广泛使用，而 Pentaho 社区在西班牙语国家非常庞大。*

现在报表已正确本地化（默认语言为英语，西班牙语），让我们通过单击工具栏上的眼睛（👁）图标来预览它。以下屏幕截图显示了默认语言（英语）的预览：

![](img/9337ac94537bf7e092e1ea6531dce8fd_494_0.png)

## 按职位标题的实际值

## 高级管理层

| 职位标题 | 实际 | 预算 |
|---|---|---|
| CEO | 500,000 | 478,750 |
| SVP Partnerships | 531,780 | 519,179 |
| SVP Strategic Development | 226,000 | 226,395 |
| SVP WW Operations | 249,800 | 249,184 |
| CEO | 500,000 | 488,750 |

如您所见，报表标题和字段的三个标题标签与静态版本相同。要更改 locale，让我们单击上部菜单中的 File | Configuration... 并检查 `.environment.designtime.Locale` 属性。完成后，将右侧文本字段填写为 `es`（西班牙语的 *i18n* 代码）并按 OK 按钮。

现在 Report Designer 配置已更新为使用西班牙语 locale，让我们通过按工具栏上的眼睛（👁️）图标再次预览报表。以下屏幕截图显示了西班牙语的预览：

![](img/9337ac94537bf7e092e1ea6531dce8fd_495_0.png)

## 按职位实际标题

## 高级管理层

| 职位标题 | 实际 | 预算 |
|---|---|---|
| CEO | 500.000 | 478.750 |
| SVP Partnerships | 531.780 | 519.179 |
| SVP Strategic Development | 226.000 | 226.395 |
| SVP WW Operations | 249.800 | 249.184 |
| CEO | 500.000 | 488.750 |

现在轮到您来处理了。例如，您可以添加法语的本地化（fr 是 i18n 代码）或使用更详细的国家/地区，例如阿根廷西班牙语（es_AR 是 i18n 代码）、智利西班牙语（es_CL 是 i18n 代码）等等。

![](img/9337ac94537bf7e092e1ea6531dce8fd_495_1.png)

> 在 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库中，具体在 Chapter 09 - Internationalization and localization/my_reports 路径下，您可以找到 report_multilanguage.prpt 报表，它完全开发了这里描述的示例。请随意使用 Report Designer 打开它并操作设置和参数，以了解如何添加和自定义国际化和本地化。

## 摘要

本章中，您学习了使用 Pentaho Report Designer 进行国际化和本地化的所有内容。您从国际化（和 i18n）的定义开始，然后了解了 Pentaho 报表中本地化的含义。为了使其更实用，您了解了如何包含资源元素（`resource-label`、`resource-field` 和 `resource-message`）以及如何使用资源包解析内容。最后，您还了解了如何测试 Pentaho 报表的各种本地化。

为了完整描述，供将来参考，您还了解了如何将现有报表本地化为英语和西班牙语（但也可以以相同方式开发其他翻译）。

现在您阅读了本章，您应该对 Pentaho Report Designer 提供的所有国际化和本地化管理方式感到自如。这是在开发复杂的 Pentaho 报表时的一项高级任务，使您成为处理可在不同语言和国家/地区使用的复杂报表的专家。

在下一章中，您将学习如何向 Pentaho 报表添加子报表和交叉表。

## 子报表和交叉表

本章中，您将学习两个高级报表主题的内在细节：子报表和交叉表。子报表允许您在主报表中嵌入可重用的报表。正如您将看到的，有了示例的帮助，子报表可以采取多种形式，从多页详细报表到包含图表的汇总子报表。作为两种特定类型的子报表，您将了解目录元素和索引元素。除了子报表之外，您还将学习如何在报表中加入交叉表。交叉表允许您在单个数据值表中比较多个变量。

本章为开发人员和信息技术人员编写的参考手册。阅读本章后，您应该对 Pentaho Report Designer 中可用的子报表和交叉表的所有高级功能感到自如。这是专门介绍 Report Designer 的最后一章，它涵盖了 Pentaho Reporting 此前端工具的最后高级功能。
