

不要认为所有场景都应使用同一数据集市；相反，应该针对每个用例思考什么才是最佳解决方案。

例如，如果你需要为 REST API 提供数据支持，最好提供按 API 所服务的数据进行切分的表。你应该尝试使用 *Snowflake Unistore* 新功能中的 *Hybrid Tables*（混合表）。

当你研究运营活动并希望分析错误情况，或当流程未按预期完成、中途停滞时，构建一些使用全外连接（full outer joins）的复合事实表可能会很有用。这样，除了常规（内）连接所能匹配到的数据之外，你还可以观察到两侧不匹配的数据。

如果你还记得我们在 *第3章* 中讨论的 *统一星型模式（Unified Star Schema）*，你可以利用 UNION ALL 运算符构建宽表，其中包含许多外键（FK），这些外键通过遵循数据中的关系来生成，从而可以基于那些看似不相关、但在其关系链中拥有某个共同外键的对象进行报表分析。

在大多数情况下，你至少会准备一些用于 BI 工具进行强力报表分析的数据集市，并提供 Kimball 星型模式。

在 *第10章* 中，我们讨论了如何以敏捷方式构建星型模式，但这里先回顾一下关键要点：

- 你的数据集市会包含许多事实表，因为它们要为使用相同维度的多个相关报表提供数据支持。
- 你应该提供所需的复合事实表，使用现有关系连接 REF 层中多个单独的基事实表，以便每个报表都能基于单一事实表构建。
- 提供维度和层级结构，使你的事实表没有孤立记录。接下来我们将通过自补全维度（self-completing dimensions）来实现这一点。
- 尽量避免将度量值放入维度中，因为这会因扇出陷阱（fan traps）而可能导致严重错误的数字。
- 确定哪些字段（这些字段也可能在维度中可用）需要作为事实的一部分提供历史值。例如，交付时的地址，因为普通维度（SCD 类型一）只会包含当前值，该值可能与历史值不同。
- 决定哪些维度（也）应包含所有变更历史，并在需要使用这些 *类型二缓慢变化维度（SCDs of type two）* 的事实中放入正确的版本外键（HDIFF 来自维度中的版本，是一个非常不错的选择）。

有关此内容的更多细节，请返回 *第10章* 查看，此外关于这方面所有细枝末节的细节，也有大量已出版的书籍可供参考。

在下一节中，我们将学习如何构建自补全维度。

最终的 CTE 通过将维度的内容与新添加的缺失条目行正确组合，构建出最终维度：

```sql
dim as (
    SELECT * FROM dim_base
    UNION ALL
    SELECT * FROM dim_missing_entries
)
SELECT * FROM dim
{% endmacro %}
```

最后一行返回补全后的维度内容。至此，该宏就可以使用了。

## 创建自补全社会维度

现在，让我们使用这个宏来创建安全维度的自补全版本。

你可以在数据集市中创建一个新文件，也可以用以下内容修改现有的 `DIM_SECURITY` 模型：

```jinja
{{ self_completing_dimension(
    dim_rel = ref('REF_SECURITY_INFO_ABC_BANK'),
    dim_key_column = 'SECURITY_CODE',
    dim_default_key_value = '-1',
    rel_columns_to_exclude =
        ['SECURITY_HKEY','SECURITY_HDIFF'],
    fact_defs = [{ 'model': 'REF_POSITION_ABC_BANK',
                   'key': 'SECURITY_CODE'}]
) }}
```

如宏的代码旁所解释的那样，我们只需传入包含维度数据的模型关系、主键列的名称，以及我们为补全维度而创建的默认记录的主键值。

`rel_columns_to_exclude` 参数是一个实用工具，可以方便地从发布的维度中移除那些在数据集中不需要的列。

需要注意的是，你可以将每个可能引入孤立记录的事实定义传递给 `fact_defs` 参数。一个事实定义由事实模型名称和指向该维度的外键字段名组成。

在我们的案例中，由于交易是根据持仓计算得出的，涉及的安全品种集合是相同的，因此并不需要同时传入两个事实，但这样做也无妨。

以下截图显示了在投资组合中引入 SAFE 持仓后，自补全社会维度的内容：

| 行号 | SECURITY_CODE | SECURITY_NAME | SECTOR_NAME | INDUSTRY_NAME | COUNTRY_CODE | EXCHANGE_CODE | RECORD_SOURCE | LOAD_TS_UTC |
|---:|---|---|---|---|---|---|---|---|
| 1 | TTD | The Trading De... | Technology | EDP Services | US | NASDAQ | SEED.ABC_Ban... | 2023-01-21 17:... |
| 2 | STAR | ISTAR INC | Real Estate | REIT - Diversifi... | US | NYSE | SEED.ABC_Ban... | 2023-01-21 17:... |
| 3 | GCM | GRAN COLOM... | Basic Materials | GOLD | CA | TSE | SEED.ABC_Ban... | 2023-01-21 17:... |
| 4 | -1 | Missing | Missing | Missing | -1 | -1 | System.Default... | 2020-01-01 00:... |
| 5 | SAFE | Missing | Missing | Missing | -1 | -1 | System.Default... | 2020-01-01 00:... |

图14.7：自补全社会维度的内容

如你所见，该维度现在有了一个主键为 SAFE 的条目，该行的内容与默认记录相同。

这一点，加上我们之前关于多个默认记录的讨论，使你能够为报表选择合适的列值。

## 恢复持仓事实以保持其数据不变

如果你预览了自补全维度但看不到带有 SAFE 主键的行，很可能是你忘记了将持仓事实恢复为完整状态。

要修复此问题，请打开 `REF_POSITION_ABC_BANK` 模型，并将最终查询改回其原始形式，如下所示：

```sql
SELECT
    p.*
  , POSITION_VALUE - COST_BASE as UNREALIZED_PROFIT
  , ROUND(DIV0(UNREALIZED_PROFIT, COST_BASE), 5)*100
      as UNREALIZED_PROFIT_PCT
FROM position as p
```

通过移除对安全维度的连接，并使用事实自身的外键，我们消除了对 REF 层的依赖，使 SAFE 外键重新出现在事实中，进而该外键也会出现在自补全维度中。

## 将测试转移到持仓事实的孤立外键上

最后但同样重要的是，我们必须将持仓事实与安全模型之间关系的测试移到交付层，该测试目前位于 REF 层。

通过移除 REF 模型中的连接，该测试可能会失败，但我们关心的是该关系在交付层是否有效且没有孤立记录，因为我们的 BI 工具将在交付层进行等值连接。

要将测试移到交付层，请先将其从 `refined.yml` 文件的 `REF_POSITION_ABC_BANK` 表部分移除。

然后，将其添加到 `_portfolio.yml` 文件中，如下所示：

```yaml
- name: FACT_POSITION
  columns:
    - name: SECURITY_CODE
      description: 'The code of the security'
      tests:
        - relationships:
            to: ref('DIM_SECURITY')
            field: SECURITY_CODE
```

上述代码将测试持仓事实与安全维度之间的关系是否存在孤立记录。

现在，我们的持仓事实完整无缺，安全维度也已补全，所有测试都应该完美通过。

## 报表中的历史——即类型二缓慢变化维度

大多数报表基于维度的当前值，因为企业希望看到公司当前名称、当前联系人、当前电话/电子邮件、当前产品名称——而不是下订单或签合同时（3个月或3年前）的名称。

到目前为止，我们所做的——从 REF 层的历史表中提取当前数据——都是为了满足这类报表需求。但是，我们存储所有变更历史的方法也使我们能够服务于那些需要查看数据在历史时刻样子的场景。

在我们的历史表中，我们已经拥有所见数据的所有版本，并且通过在 STG 层创建 HDIFF 列，我们也为这些版本创建了唯一键。

我们的历史表高效地存储了构建更正式称为 **类型二缓慢变化维度** 所需的数据。

通过从 HIST 表提取数据，并且对每个 HDIFF 只取一份副本（请记住，相同内容可能会多次出现并被存储），你可以构建一个包含实体所有版本的 SCD2 维度。

要使用这样的维度，你必须根据事实日期以及特定 HKEY 的不同 HDIFF 的有效期，将正确的 HDIFF 填充到你希望连接该维度的事实中。

你可以使用 LAG 函数计算每个 HDIFF 的有效期，类似于我们根据持仓计算交易的方式。

要填充 HDIFF，你可以从提供维度外键和某个相关日期的事实开始，然后基于主键和有效期范围在 SCD2 维度和该日期中查找对应的 HDIFF。

