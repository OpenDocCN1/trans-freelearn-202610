## 使用落地表加载模式

到目前为止，你已经知道如何设置所需的对象、创建落地表并将数据摄取到落地表中。

为了简单可靠地执行此操作，创建一个 `load_all_xxx` 宏，该宏将调用设置宏，然后调用所有 `load_yyy` 宏以加载各个表。

`load_all_xxx` 宏的内容如下所示：

```jinja
{% macro load_all_xxx() %}
{{ log('*** Load for xxx system ***', true) }}

{{ log('**  Setting up for XXX system **', true) }}
{% do run_query(setup_xxx_sql()) %}

{{ log('*  load_YYYYY *', true) }}
{% do load_YYYYY() %}

--
{{ log('*** DONE loading for xxx system ***', true) }}
{%- endmacro %}
```

首先，你必须调用设置宏，然后添加对每个表加载宏的调用。

完成此操作后，你可以通过将以下命令添加到作业中来安排执行：

```text
dbt run-operation load_all_xxx
```





你可以选择按顺序为每个源系统安排摄取，每个系统使用一个 `load_all_XXX` 宏，也可以并行执行然后安排转换。

无论如何，你将通过将它们定义为暂存模型的源来从落地表中读取。

另一个实际考虑因素，尤其是在你的源每天都有完整导出时，就是删除已从落地表中加载的旧数据。

要实现这一点，你可以在 COPY INTO 命令后简单地添加一个 DELETE 查询。DELETE 查询看起来像这样：

```
DELETE FROM landing_table
WHERE <where_clause>
```

对于 WHERE 子句，有几种方法可以遵循：

- 保留最后 *N* 次摄取批次。每次我们向落地表加载数据时，所有行的 `INGESTION_TS_UTC` 相同，因此你可以编写如下子句：

    ```
    WHERE  INGESTION_TS_UTC  NOT  IN
        (SELECT DISTINCT INGESTION_TS_UTC
           FROM <landing>
           ORDER BY INGESTION_TS_UTC DESC
           LIMIT N)
    ```

- 删除摄取时间超过 *N* 天的数据。在这里，你可以编写如下子句：

    ```
    WHERE
        INGESTION_TS_UTC  <  DATEADD(DAY, -N, CURRENT_DATE)
    ```

- 你也可以编写一个 WHERE 子句，引用将存储落地表数据的 HIST 表，并删除在 HIST 表中 `INGESTION_TS_UTC` 最大值之前加载的数据。

  虽然诱人且 SQL 允许这样做，但这不是一个好主意：你会创建一个循环，其中落地表依赖于 HIST 表，而 HIST 表又依赖于落地表。这会使运行和维护项目变得复杂。

请注意，虽然在列式数据库中删除通常相当慢，但如果你基于 `INGESTION_TS_UTC` 进行删除，你基本上是按照每次日常加载时创建的微分区进行删除，因此你将获得非常好的性能。
