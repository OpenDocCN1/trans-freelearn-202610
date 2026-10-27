

为了报告当前投资组合，我们只想读取当前有效的持仓记录。

为此，我们需要从快照中仅选择尚未关闭的行——即 `DBT_VALID_TO` 字段为 null 的行——因为旧版本在“DBT_VALID_TO”列中带有时间戳。

REF_POSITION_ABC_BANK 模型现在看起来像这样：

```
WITH
current_from_snapshot as (
    SELECT *
    FROM {{ ref('SNSH_ABC_BANK_POSITION') }}
    WHERE DBT_VALID_TO is null
)
SELECT
    *
    , POSITION_VALUE - COST_BASE as UNREALIZED_PROFIT
    , ROUND(UNREALIZED_PROFIT / COST_BASE, 5)*100
        as UNREALIZED_PROFIT_PCT
FROM current_from_snapshot
```

可以看到，在 `current_from_snapshot` CTE 中，我们添加了 WHERE 子句，以便从快照中只读取尚未关闭的行。

现在我们已经更改了 REF 模型，可以使用以下 dbt 命令重建它及其上游模型：

```
dbt build -s +FACT_POSITION
```

此命令将对源数据运行测试、刷新快照，并重建 REF 层和数据集市模型。

我们现在已经重构了代码，使其更具弹性、更易于维护、更容易理解和演进。

我们构建的模型只是用于输出一些报表的最小数据模型，但在这些报表中，我们甚至无法获取账户所有者的姓名。

为了改善这种情况，我们需要开始添加维度数据，这将在下一章中进行。

## 小结

干得漂亮！

在本章中，你仅使用 dbt 的基本功能就构建了一个可用的数据平台，其架构合理、组织清晰。

你现在已经理解了架构的所有层次，能够为每一层构建合适的模型，并使用 dbt 快照将更改保存到数据中。

在下一章中，我们将学习如何将维度数据整合到架构中，并巩固本章学到的所有内容。

## 重要提示

请注意，在本章中，我们一直专注于向你介绍每个架构层中合适的架构和模型组织的基础知识。为了控制篇幅并保持重点，我们有意忽略了一些重要的活动，例如添加测试来验证输入源数据和我们在数据集中市中发布的数据。请不要认为这些活动不重要。

我们还使用了一些基本功能，因为尚未介绍用于实现更高级解决方案的特性。我们将在接下来的章节中介绍它们，并重构我们的解决方案来使用这些特性。

## 使用维度数据

在上一章中，我们从零开始探索了构建数据平台背后的核心原则，并在参考架构中实现了这些原则。

在本章中，我们将学习如何将维度数据整合到数据模型中，并利用它来扩展描述业务事实的事实表。维度数据使事实信息对业务用户来说更加有用且易于使用。

**维度数据**一词指的是为实体提供可读信息的描述性数据，例如客户或账户的名称或国家。

我们将探索如何创建数据模型来规划工作，并为 *Pragmatic Data Platform* 架构的所有层构建 dbt 模型，以在数据集中市中交付维度数据。

最后，我们还将借助一个练习来回顾上一章和本章所学的内容。

本章涵盖以下主题：

- 添加维度数据
- 加载第一个维度的数据
- 为第一个维度构建 STG 模型
- 保存维度数据的历史记录
- 使用维度数据构建 REF 层
- 将维度数据添加到数据集市
- 练习——添加更多手动维护的维度

### 添加维度数据

一般来说，维度数据用于通过事实表中存储的维度实体代码，去连接维度表以获取描述性信息，从而为事实提供描述性信息。

我们在上一节加载的持仓事实有四个显式外键，我们恰当地用 `_CODE` 后缀命名：账户代码、证券代码、交易所代码和货币代码。

这四个代码是引用，或者说外键，指向我们可以直接连接到该事实的四个维度。

还有一个额外的隐式维度——银行维度，它隐含在模型的名称中。

### 为精炼层和数据集市层创建清晰的数据模型

为了能够最终确定维度和事实表设计，我们需要对报表（数据集市层）中使用的数据模型有一个清晰的了解，这通常是星型模式，或者很少情况下是一张宽表。

这反过来又与我们所拥有且能够在精炼层完整表示的数据模型紧密相连并受其影响。

让我们先快速看一下简化后的概念数据模型，它涉及我们已命名并希望围绕持仓事实进行报表展示的实体：

![](img/8868d272a75eee8b407a8e0f3434d10f_280_0.png)

图 7.1：示例项目的概念数据模型

在此数据模型中，我们看到了前面提到的四个维度，以及我们讨论过的隐含银行维度。

### 加载第一个维度的数据

在本节中，我们将了解如何创建（如果你没有来自某个外部源系统的数据）并加载证券维度的维度数据，该维度描述了账户持仓中包含的证券。

在本节结束时，我们将邀请你将其他维度作为练习添加进来。

### 创建 CSV 文件并作为 seed 加载

在真实的银行数据平台中，证券维度很容易达到几十列，其中一些列是不经常变化的信息值，而大多数列则是根据交易活动计算出来的值。

在本节中，我们将创建一个简单的 CSV 文件，其中包含几个基本列：`Security_Code`、`Security_Name`、`Sector`、`Industry`、`Country` 和 `Exchange`。

| SECURITY_CODE | SECURITY_NAME | SECTOR | INDUSTRY | COUNTRY | EXCHANGE |
|---|---|---|---|---|---|
| TTD | The Trade Desk, Inc. | Technology | EDP Services | US | NASDAQ |
| GCM | GRAN COLOMBIA GOLD CORP | Basic Materials | GOLD | CA | TSE |
| STAR | ISTAR INC | Real Estate | REIT - Diversified | US | NYSE |

*图 7.3：表示 ABC_BANK_SECURITY_INFO.CSV 文件内容的 Excel 表格*

CSV 文件的内容如下所示：

```
SECURITY_CODE,SECURITY_NAME,SECTOR,INDUSTRY,COUNTRY,EXCHANGE
TTD,"The Trade Desk, Inc.",Technology,EDP Services,US,NASDAQ
GCM,GRAN COLOMBIA GOLD CORP,Basic Materials,GOLD,CA,TSE
STAR,ISTAR INC,Real Estate,REIT - Diversified,US,NYSE
```

你可以在 Excel 等电子表格中创建自己的 CSV 文件，如前面的截图所示，然后将其导出为 CSV；也可以直接在 dbt 中创建 CSV 文件，然后键入或复制粘贴内容（注意换行）。

如果你不想手动输入内容，可以从我们的 GitHub 仓库的 *Chapter 6* 文件夹中下载，文件名为 `06_05_v1__ABC_Bank_SECURITY_INFO.csv`。

让我们创建 CSV 文件：

- 1. 打开 dbt Cloud 文件**资源管理器**中的 seeds 文件夹。
- 2. 打开右侧的菜单（三个点），点击**创建文件**。
- 3. 在弹出的窗口中输入文件名 `ABC_Bank_SECURITY_INFO.csv`。
- 4. 打开新文件并编辑其内容，使其与本部分所示内容（前面的代码和下面的截图）一致：

![](img/8868d272a75eee8b407a8e0f3434d10f_283_0.png)

### 配置 seed 并加载它们

在 seeds 文件夹中创建好 CSV 文件后，就该配置希望 seed 数据加载的位置，然后执行加载。

让我们来操作：

- 1. 打开主配置文件 `dbt_project.yml`，并在文件末尾添加以下代码：

```
# Configuring Seeds
seeds:
  +schema: seed_data
```

此配置指示 dbt 将所有 seed 加载到名为 seed_data 的 schema 中。

- 2. 配置好希望 dbt 创建包含 CSV 数据表的位置后，就可以运行命令加载数据了。在 dbt 命令行中执行以下命令：

```
dbt seed
```

运行 `dbt seed` 命令后，你会发现 dbt 在配置的 schema 中创建了一个与 CSV 文件同名的表，并带有您开发环境常用的前缀。

### 为 seed 添加数据类型和加载时间戳

在 seed 加载过程中，我们很幸运，所有数据都是字符串类型，并且没有 null 值，因此即使在未指定列数据类型的情况下，一切也都顺利进行了。

加载表之后，我们还注意到，我们无法知道数据是什么时候加载的。

