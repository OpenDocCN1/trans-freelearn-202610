

## 数据来源

图 12.29：按系统划分的数据源新鲜度报告

数据源新鲜度报告提供了一种快速简便的方式来检查传入数据是否存在问题，并且结合 dbt 在作业失败时发送警报的能力，当表的 SLA 未满足时发送警报，将错误通知给负责的人员/团队。操作非常简单，只需创建一个仅执行新鲜度检查并配置失败时需要通知的人员的作业即可：

图 12.30：按系统划分的新鲜度报告

我们示例项目的报告显示，我们定义了两个系统，每个系统都定义了一个表。**abc_bank** 系统存在一些陈旧数据，而对于 **seeds** 系统，所有数据都是新鲜的。

您可以在 YAML 文件中声明源系统时设置 SLA，通过设置阈值级别并命名时间戳列：

```yaml
sources:
  - name: seeds
    freshness:
      warn_after: {count: 3, period: day}
      error_after: {count: 5, period: day}
    loaded_at_field: LOAD_TS
```

如前面的代码所示，您可以提供警告级别和错误级别，并且必须命名要与当前时间进行检查的时间戳列。

与大多数 dbt 配置一样，您可以在系统级别设置通用 SLA，并为每个源表覆盖阈值和时间戳列。

如果您设置了系统范围的 SLA，您可以通过将新鲜度设置为 **null** 来禁用不需要检查的表的新鲜度检查（例如很少更改的参考表），正如我们在以下片段中对 t2 表所做的那样：

```yaml
tables:
  - name: t1
  - name: t2
    freshness: null   # no freshness for this table
```

数据源新鲜度报告可通过 **Deploy** 菜单的最后一个条目访问。

![](img/8868d272a75eee8b407a8e0f3434d10f_441_0.png)

图 12.31：包含 Data Sources 条目的 Deploy 菜单

新鲜度报告也对使用读者许可证访问 dbt 的非开发人员用户可用，使其可供业务用户使用，同时提供生成的文档。

## 暴露

Exposure 在依赖关系图中添加了一个终端节点，用于描述对某些特定 dbt 模型的外部依赖。

在 exposure 的核心，是一个 dbt 模型的列表，暴露的依赖项依赖于这些模型。

exposures 的主要好处如下：

- 为外部功能所依赖的列出模型提供一个清晰的名称，以便简单且一致地应用 dbt 命令
- 能够为报告添加手动维护的文档到文档站点，并直接访问 exposure 所依赖的模型

以下代码为我们由唯一的事实表和唯一的维度表组成的简单报告创建了一个 exposure：

![](img/8868d272a75eee8b407a8e0f3434d10f_442_0.png)

图 12.32：为我们的示例项目定义 exposure 的代码

让我们分析一下：

- 您通过提供名称来创建 exposure，因此名称是强制性的。

  名称必须采用驼峰命名法，因为它可以用作任何 dbt 节点名称。

- 您还需要提供外部依赖的类型。

  外部依赖可以是任何东西，但可用的类型有 `analysis`、`application`、`dashboard`、`ml` 和 `notebook`。

  类型用于在文档站点中组织 exposures，它们按类型分组。

- 最后一个强制字段是报告所有者的 `email` 字段，而所有者的姓名是可选的。

- 最重要的是提供 `depends_on` 列表。

  虽然是可选的，但它是创建 exposure 的主要原因，除非您只是想要一个可以在文档中手动编辑的页面。

  您可以使用 `ref` 和 `source` 函数来构建列表，但您很少需要 `source` 函数。

  在列表中，您只需列出外部功能所依赖的最终模型；构建它们所需的其他模型 dbt 会通过节点的 DAG 得知。在前面的屏幕截图中，我们列出了数据 mart 层的两个模型，dbt 显示了完整的血缘图。

- exposure 的另一个关键组件可以是描述。

  虽然您可以只输入一些简单文本，但当与 Markdown 文档块一起使用时，它成为提供易于使用且视觉上引人注目的文档的非常强大的方式。

- exposure 节点现在将出现在所有血缘图中，无论是在 IDE 中还是在文档站点中的生成文档中。

前一个 exposure 定义的结果显示在以下屏幕截图中，该截图通过使用 Markdown 文档块增强了报告内容的描述：

图 12.33：exposure 的生成文档

在为 exposure 生成的文档页面中，您可以看到指向所有者的详细信息、通过使用 Markdown 文档块增强外观的描述，以及模型列表，只需点击一下即可访问。

血缘图也只需点击一下即可访问，其布局与 IDE 屏幕截图中看到的相同。

exposures 在左侧导航中有专用的菜单，按类型分组。

## Markdown 文档

在讨论 exposures 时，我们展示了使用 Markdown 文档块的结果。我们在谈论什么？

在 dbt 中，您可以在 YAML 文件中描述的大多数元素中声明 description 字段。每当您可以输入描述时，您也可以使用文档块定义全部或部分内容。

文档块是具有唯一名称的 docs Jinja 对象的定义，其中包含在文档站点中可视化时被解释为 Markdown 的文本：

```
{% docs exp_portfolio_report %}
# Portfolio report
The Portfolio report features are:
* list all positions by account
* provide up to date valorization of positions.
  The value is available
  * in the position currency, depending on the exchange
  * in the account base currency, as set by the owner
{% enddocs %}
```

我们在 exposure 示例中使用的前一个文档块必须位于以 .md 结尾的文件中。它可以与您的模型位于同一文件夹中，名称由您决定。同一文件可以包含多个文档块。

要使用文档块，您可以使用 doc 函数以及块的名称：

```
description: |
  The basic portfolio report. Positions by account.
  {{ doc('exp_portfolio_report') }}
```

前面的描述混合了普通文本和 doc 函数，使用 | YAML 块表示法，表示对多行 YAML 字符串的“逐字”解释。

这很重要，因为 Markdown 中空行很重要，如果您改用 > YAML 折叠样式，新行将被空格替换，从而改变您的 Markdown，因此渲染输出也会改变。

您也可以直接在表的描述字段、列或任何可能的位置中使用 doc 函数，但在此情况下，您可能需要引用 doc 函数：

```
description: "{{ doc('exp_portfolio_report') }}"
```

每当您引用一个 Jinja 块时，请使用与用于文档块名称字符串的引号相反类型的引号。

请注意，您可以直接在描述中使用 Markdown，而无需创建文档块。当您想要包含链接时，这很有用，如以下示例所示：

```
description: "See [the docs](http://www.docs.com)"
```

提供易于使用且令人愉悦的文档可以提高您的高级用户充分利用您准备的数据集的能力。dbt 允许您按您认为合适的方式提供文档。

## 总结

恭喜，您已完成本书的第二部分，现在了解了所有核心 dbt 功能以及如何使用它们！

在本章中，您学习了如何部署和运行您的项目，以及如何使用高级自动化功能来扩展 dbt 以执行您所需的所有任务。

您现在熟悉 dbt 功能及其广泛的文档功能，并且可以使用它们来提高项目的价值。

在下一章 *Moving Beyond Basics* 中，我们将开始介绍更高级的数据工程主题，例如身份和主数据管理或保存实体的更改，以及我们用于处理它们的 dbt 功能。

