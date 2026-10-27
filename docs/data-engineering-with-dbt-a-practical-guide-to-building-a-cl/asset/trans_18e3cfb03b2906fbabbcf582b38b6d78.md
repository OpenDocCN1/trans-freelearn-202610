

我们随后声明了第一个测试，`not_null`，要求表中每一行的 ACCOUNTID 列不包含 `null` 值。请注意，这与数据库列是否被声明为可空无关。

`not_null` 测试在很大程度上是最常用的测试，因为我们经常希望确保我们获取或产生的数据在被测试的列中始终具有非空数据。

> **实用提示**
>
> 在我们转向为此数据源想要介绍的其他测试之前，这里有一些关于处理 YAML 文件的实用建议。
>
> 请注意在 YAML 文件中，对齐对于定义结构至关重要，因此请尽量精确地进行，否则您会遇到编译错误，因为 YAML 解释器将无法正确解析您的配置文件。
>
> 在添加新的配置片段后，请检查 dbt IDE 右下角是否显示**就绪**的绿色字样，以确保项目编译正确。为了确保您不会引入编译错误，您可以在保存更多更改时瞥一眼。
>
> 当这种情况发生，即使在阅读错误消息后您也无从知晓时，我们建议删除您更改的块，以便编译再次正确，然后一次添加回一点，以了解阻止编译的问题所在。

让我们继续添加并对表中其他列所需的测试进行评论：

```yaml
- name: SYMBOL
  description: The symbol of the security at ABC Bank.
  tests:
    - unique
    - not_null
```

接下来，我们正在配置 SYMBOL 列的描述和测试，我们假设它是一个银行头寸的良好自然键。

因此，我们同时声明了 `not_null` 和 `unique` 测试，这是唯一性约束的标志，例如键，无论是自然键还是代理键。

```yaml
- name: DESCRIPTION
  description: The name of the security at ABC Bank.
  tests:
    - not_null
```

在前面的配置片段中，我们正在配置 DESCRIPTION 列的描述和测试，我们希望它为 not_null，因为它具有证券的名称。

```yaml
- name: EXCHANGE
  description: >
    The short name or code of the exchange
    where the security is traded.
  tests:
    - not_null
```

在前面的配置片段中，我们正在配置 EXCHANGE 列的描述和测试，我们希望它为 not_null，因为它具有证券被购买的交易所的名称或代码。我们将在参考 STG 模型时更深入地讨论命名。

我们也可以考虑使用 accepted_values 测试来表达交易所值应限制为已知值列表，但由于我们没有理由限制值，此类测试只会在失败时产生误报，促使我们维护列表以添加新值。

请注意，如果我们需要因任何法规而被提醒使用新交易所，或确保其他数据或授权已到位，这可能是一个明智的策略。

然后我们会将授权或配置的交易所添加到测试接受的列表中，如果使用我们未命名为已接受的其他交易所，我们将收到错误。

```yaml
- name: REPORT_DATE
  description: >
    The date the report was extracted by ABC Bank.
    We consider this position to be effective
    from this date forward, until a change is seen.
  tests:
    - not_null
```

对于 REPORT_DATE 列，我们希望它为 not_null，因为我们需要能够知道位置中的信息所指的日期。时间在现实中非常重要，在数据仓库方面，这可能是整本书的主题。我们将经常谈论它。

```yaml
- name: QUANTITY
  description: >
    The number of securities we hold in the
    portfolio at ABC Bank at the Report Date.
  tests:
    - not_null
```

对于 QUANTITY 列，我们希望它为 not_null，因为我们需要知道我们有多少证券。仅知道“投资组合中有一些该证券”而不知道实际持有的股票或头寸数量根本没有帮助。

```yaml
- name: COST_BASE
  description: >
    The cost we paid for the securities that
```

您可以在我们的存储库中找到此源文件的更新版本，在 dbt 文件夹中第 5 章的材料中，05_03_v2__source_abc_bank.yml 文件内。

## 重要说明

测试在整个数据生命周期中非常有用。

在这里，我们在创建源和第一个模型之后才介绍它们，以避免用太多新主题使您不堪重负。在项目的现实中，在编写源和模型时编写测试是有用的。

当开始使用我们不完全了解的数据时，测试尤其有用，因为它们允许数据工程师轻松快速地表达和验证他的假设。

知道预期的唯一性约束和字段的可空性，特别是外键和关系完整性约束是否得到满足，对于设计数据上的 ELT 转换非常重要。

测试使我们能够最初分析，然后完全描述，最后不断监控传入数据的质量，使我们的 ELT 更加可靠，结果值得信赖。

## 运行测试

现在我们已经添加了第一个测试，我们如何使用它们？

在 dbt 中，您使用 `dbt test` 命令执行测试，与我们之前看到的运行 dbt 模型的方式非常相似。

转到 dbt Cloud 命令行并运行以下命令：

```
dbt test
```

前面的命令将执行 dbt 项目中的所有测试，您会看到命令行区域打开以首先显示执行，然后显示测试结果：

![](img/8868d272a75eee8b407a8e0f3434d10f_235_0.png)

您会看到您的测试正在运行，并且完成后，如果通过，右侧会有绿色勾号，如果产生警告，则为橙色感叹号，如果失败，则为红色“x”符号。

您可以打开每个测试的行以访问摘要结果，详细说明测试结果及其持续时间，以及详细信息，您还可以看到在 Snowflake 数据库上运行的确切查询。

现在，我们只为单个源表定义了测试，因此运行所有测试完全没问题，但展望未来，能够仅运行您想要的源或模型的测试非常重要。

以下命令为您提供了更好地控制运行测试的替代方案：

- 运行所有源的测试：

  ```
  dbt test -s source:*
  ```

- 运行 abc_bank 源中所有表的测试：

  ```
  dbt test -s source:abc_bank.*
  ```

- 运行 abc_bank 源中 ABC_BANK_POSITION 表的测试：

  ```
  dbt test -s source:abc_bank.ABC_BANK_POSITION
  ```

在接下来的章节中，我们将更深入地了解 dbt 提供的模型选择语法，以便您可以构建运行或测试您想要的确切模型的命令。

## 测试我们交付的信息是否可靠

现在我们确信传入的数据符合我们的期望，是时候确保我们交付的内容也符合期望了。

编写验证我们想要交付内容的测试将在许多方面为我们提供帮助：

- 确保我们第一次编写代码时做对了。
- 它提供了一个清晰且非常有价值的文档片段，它将随着我们的代码保持最新，或在代码或数据停止遵守它时失败并引起我们的注意。

  在这方面，重要的是要注意，测试为记录项目提供了如此宝贵的信息，以至于 dbt 使用它们来丰富项目文档。
- 它将继续监控我们或他人对代码所做的每一次未来更改是否仍将满足我们的期望以及我们的客户所使用的合同。

我们已在 `refined` 文件夹中创建了模型，因此让我们在 `refined` 文件夹中创建一个名为 `refined.yml` 的 YAML 文件来托管我们的测试以及我们在 `refined` 文件夹中实现的转换的描述。

与往常一样，在我们的 YAML 文件中，让我们从版本和我们将在文件中描述的实体类型开始。用这两行开始文件：

```yaml
version: 2
models:
```

版本始终为 2，因为这是当前使用的 YAML 文件的定义 - 然后，我们使用 `models` 关键字声明我们将描述一些模型。

与源一样，首先，我们使用 name 条目标识我们想要描述和测试的模型，然后我们可以输入描述，除了帮助我们作为开发人员外，还将用于文档中：

```yaml
- name: POSITION_ABC_BANK
  description: The positions we have in the ABC Bank portfolio.
```

嵌套在模型内部，`columns` 允许我们命名我们想要描述和测试的列：

```yaml
  columns:
    - name: UNREALIZED_PROFIT
      description: The unrealized profit on the position.
      tests:
        - not_null
```

