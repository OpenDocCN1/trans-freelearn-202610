

前面的图展示了我们在两种形式下看到的连接类型：

1. 仅使用 ON 子句，展示纯连接的结果
2. 在 ON 子句所用列上使用 WHERE 子句

在图中，这些信息被用来从外连接中排除发生匹配的行，即 A 和 B 重叠的区域，也就是内连接返回的行。

这类查询非常有用且常用，例如，用于检查我们是否拥有引用了客户表中不存在的客户的订单。这被称为订单表中的孤立键。

让我们看一个使用 Snowflake 示例数据的例子：

```sql
SELECT *
FROM "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF1"."ORDERS"
LEFT OUTER JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF1"."CUSTOMER"
    ON C_CUSTKEY = O_CUSTKEY
WHERE C_CUSTKEY is NULL;
```

此查询应不返回任何行，因为订单引用的所有客户都应存在于客户表中。现在我们已经涵盖了 SQL 中的所有基本函数，让我们来了解一个高级特性：窗口函数。

## 高级——引入窗口函数

窗口函数是一种 SQL 函数，它作用于一组行，称为行窗口。

这些函数也被称为分析函数，因为它们主要用于分析型查询中。

窗口是一组相关行，包含从 0 到查询结果中的所有行的任意数量。

通常，窗口会将结果行划分为不相交的分区。

某些窗口函数还可以作用于窗口框架，这是窗口定义的行中的一个动态子集，基于当前行。

您可以定义一个累计窗口框架，取窗口中当前行之前或之后的行，或者定义一个滑动窗口框架，从当前行之前的若干行延伸到当前行之后的若干行。

窗口函数与使用 GROUP BY 的聚合函数有很多共同点，因为它们都将查询结果划分为组（即分区），然后对每个组中的行计算函数值。

要使用窗口函数，可以使用以下语法：

```
<function_name> ([<arguments>])
    OVER ( [PARTITION BY <part_expr>] [ORDER BY <order_expr>])
```

OVER 子句的存在指定了该函数应用于 OVER 子句内定义的窗口。当然，函数也可以接受自己的参数。

PARTITION BY 子句定义了行如何分区，`<part_expr>` 是一个由列或基于查询数据计算的表达式组成的逗号分隔列表，这与我们之前看到的 group by 表达式类似，如前面的示例所示。

ORDER BY 子句指定了用于计算函数的行的排序方式，且该排序在窗口函数之外不可见。ORDER BY 子句和 `<order_expr>` 的工作方式与 SELECT 语句中对查询结果排序时基本相同。

在语法层面上，PARTITION BY 和 ORDER BY 子句都是可选的，即使某些函数要求或禁止这两者之一。请查阅每个函数的文档以了解更多细节。

ORDER BY 子句对顺序敏感的窗口函数有影响，这些函数分为两种类型：

- **排名相关函数**：这些函数根据行的排名（即行在窗口中的顺序）来计算值。它们用于将行在窗口中的多种可能排序之一显式化，即固化为一个以后可以使用的值。

  一个例子是根据积分或奖牌对锦标赛中的队伍进行排序：当出现相同值时可以有不同的排序风格，具体取决于你如何决定。

  例如，如果两支队伍（A 和 B）得分相同，而另一支队伍（C）得分更高，你可以说 A 和 B 都是第二名，也可以说一支是第二，另一支是第三。

- **窗口框架函数**：这些函数作用于窗口框架，即窗口行的动态子集。如前所述，它们可以使用累计或滑动窗口框架。下一节将深入探讨这些内容。

## 窗口框架定义

某些函数可以使用动态窗口框架，而不是固定窗口。

您在窗口的 OVER 子句中，在窗口的 ORDER BY 子句之后指定窗口框架：

```
<function_name> ([<arguments>])
    OVER ( [PARTITION BY <part_expr>]
            ORDER BY <order_expr>
            <cumulative_frame_def> | <sliding_frame_def>
    )
```

**累计窗口框架**是窗口的一个子集，它根据窗口内的当前行而增长或收缩。框架包含窗口中当前行之前或之后的行。累计框架定义的语法如下两行：

```sql
{ROWS | RANGE} BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
| {ROWS | RANGE} BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
```

第一个表达式取分区中的所有行，或同一排序范围内截至当前行的所有行，而第二个表达式则取从当前行到分区或范围的末尾的所有行。接下来会更详细地描述 ROWS 和 RANGE。

**滑动窗口框架**是一个行子集，它从当前行之前的 N 行延伸到当前行之后的 M 行。一侧或两侧可以固定到分区的开始或结束。

以下是该语法的三种可能形式：

```sql
ROWS BETWEEN <N> { PRECEDING | FOLLOWING }
          AND <M> { PRECEDING | FOLLOWING }
| ROWS BETWEEN UNBOUNDED PRECEDING AND <M> { PRECEDING | FOLLOWING }
| ROWS BETWEEN <N> { PRECEDING | FOLLOWING } AND UNBOUNDED FOLLOWING
```

第一种语法用于当你希望创建一个从当前行之前的 N 行延伸到当前行之后的 M 行的框架时，而另外两种是一侧固定为窗口起点或终点的情况。

### 累计窗口定义中的 ROWS/RANGE

在指定累计窗口框架时，我们可以使用 ROWS 或 RANGE，但它们的区别是什么？

- **ROWS**：表示框架的固定一侧延伸到分区的边界：从分区起点到当前行，或从当前行到分区末尾。
- **RANGE**：表示框架的固定一侧延伸到与当前行在 ORDER BY 值上相同的前行/后行。框架作用于排序中处于相同位置的 precedding/following 行。例如，你可以想象在锦标赛列表中依次经过许多积分相同的队伍。

> **重要说明**
>
> 提供有关窗口函数的广泛信息超出了本 SQL 分析操作入门指南的范围。这也是 SQL 在不同数据库中表现并不总是一致的一个领域。我们建议在您所用数据库的文档中搜索“window functions”。

## 总结

在本章中，您学习了 SQL 的基础知识，并从基础逐步深入到一些相当高级的主题。您还亲身体验了在云端使用 Snowflake 的过程。

您了解了不同的 SQL 命令和类别，在 Snowflake 中开设了账户，并使用用户和角色创建了您的第一个数据库。随后，您详细学习了编写 SELECT 查询时可能用到的各种子句和运算符，并使用 Snowflake 示例数据进行了尝试。然后，您更详细地分析了如何使用 JOIN 子句来混合多个表中的数据。我们在本章结束时介绍了窗口函数，它为我们的查询开辟了新的途径，并使许多复杂的计算变得简单高效。

在下一章《设置您的 dbt Cloud 开发环境》中，我们将创建第一个 dbt 模型，并在云端设置 dbt 和 GitHub。

## 进一步阅读

在本章中，我们描述了最重要的 SQL 主题，但如果您想更深入地学习并精通 SQL，还有更多内容可以阅读。

Snowflake 文档可免费使用，提供了良好的解释和示例，当然也是 Snowflake SQL 方言的参考知识来源。

您可以从以下页面开始：

- SQL 命令参考：https://docs.snowflake.com/en/sql-reference-commands.html
- 命令摘要：https://docs.snowflake.com/en/sql-reference/intro-summary-sql.html
- 查询：https://docs.snowflake.com/en/user-guide/querying.html

