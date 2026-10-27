## 从半结构化文件加载

摄取半结构化文件的工作方式与摄取 CSV 文件几乎相同，唯一的区别是，与其仅使用位置列的列表（$1, ... $n），我们需要为要读取的每个列提供一个表达式。

该表达式必须提供半结构化文件模式中信息的路径，并且还要将表达式转换为所需的 SQL 类型。

我们之前已经看到如何使用 INFER_SCHEMA 函数获取读取半结构化文件数据的表达式。

考虑到与 CSV 摄取的唯一区别是需要提供读取字段的表达式，我们的宏中的更改如下：

- 用字段表达式的定义替换字段数量
- 使用字段定义而不是生成位置列名

让我们来看一下文件中以我们要摄取的表命名的两个宏，从加载宏开始：

宏声明与 CSV 的一样，但没有字段数量。现在，我们需要提供读取字段的完整表达式，而不仅仅是指定它们的数量：

```sql
{% macro load_ZZZ(
    db_name = get_XXX_ingestion_db_name(),
    schema_name = get_XXX_ingestion_schema_name(),
    stage_name = get_XXX_ingestion_stage_name(),
    format_name = None,
    table_name = 'ZZZ',
    pattern = '.*ZZZ[.]parquet'
) %}
```

完整表名的创建以及对创建落地表的宏的调用与 CSV 摄取相同：

```sql
{% set full_table_name =
        db_name ~ '.' ~ schema_name ~ '.' ~ table_name %}

{% do run_query(X_Z__create_table_sql(full_table_name)) %}
{% set field_expressions %}
    $1 as src_data,
    $1:ZZZ_CODE::string as ZZZ_CODE,
    $1:ZZZ_NAME::string as ZZZ_NAME,
    $1:ZZZ_VALUE::double as ZZZ_VALUE
{% endset %}
```

接下来，我们使用 set 块创建了一个变量，其中包含读取半结构化数据字段的表达式。请注意，你可以自己编写它，也可以由 INFER_SCHEMA 和 GENERATE_COLUMN_DESCRIPTION 函数或一些 SQL 生成，就像我们在本章前一节中展示的那样。

我们最后通过调用 `ingest_semi_structured_into_landing_sql` 宏并将字段表达式作为字段数量的替代传递来完成：

```sql
{% do run_query(ingest_semi_structured_into_landing_sql(
            full_table_name,
            field_expressions,
            file_pattern = pattern,
            full_stage_name = stage_name,
            full_format_name = format_name
        ) ) %}
{%- endmacro %}
```

第二个宏是创建落地表的宏——它与 CSV 情况相同，但请记住，现在你必须使用与 `field_expressions` 变量中相同的名称和数据类型来定义列。

唯一的特点是，如果你想以 variant 形式存储源数据，就像我们在本示例中所做的那样，将 $1 作为 src_data 作为第一个表达式。然后，你需要为该列创建第一个字段，并使用 variant 数据类型：

```sql
-- Add declaration for table columns
    SRC_DATA variant,
    ZZZ_CODE string NOT NULL,
    ZZZ_NAME string,
    ZZZ_VALUE double,
```

前面的片段举例说明了本节中 `field_expressions` 变量示例中定义的字段的字段声明。

请注意，通过以 variant 形式将 $1 存储在源数据中，你正在复制存储的数据大小，因此它只有在你在使用 variant 提取数据时遇到困难的初始开发阶段才有意义，或者如果你决定提取很少的字段并将大部分数据作为 variant 存储作为主要存储。

请注意，当解包和展平半结构化实体的模式非常复杂或会带来对象关系转换问题时，这可能是一个合理的解决方案。

一个典型的例子是解包带有标题和行的文档。这是半结构化文件中存储的 **嵌套分层数据** 的常见示例。

如果行没有稳定的唯一标识符，而只是由标题 ID 加上行号标识，这在订单或发票中经常这样做，那么就无法单独跟踪行（想想如果一行被删除，而其他行没有变化该怎么办），因此最好将文档的版本作为一个整体进行跟踪，然后在 ref 层中解包它。

另一种选择是展平标题和行，但为行创建 HDIFF，这样如果发生任何更改（标题或行），所有现有行都将保存与更改的标题相同的 LOAD_TS，并且可以使用 QUALIFY 子句与按 HKEY 标题分区并按 LOAD_TS 降序排序的 rank 函数检索所有行。rank=1 将检索在最后 LOAD_TS 编写的全部行。

现在我们已经看到了半结构化数据的加载宏，让我们快速看一下实现加载模式的宏——`ingest_semi_structured_into_landing_sql` 宏：

```sql
{% macro ingest_semi_structured_into_landing_sql(
        full_table_name, field_expressions,
        file_pattern, full_stage_name, full_format_name
) %}
BEGIN TRANSACTION;
COPY INTO {{ full_table_name }}
FROM (
    SELECT
        {{ field_expressions }},

        METADATA$FILENAME as FROM_FILE,
        METADATA$FILE_ROW_NUMBER as FILE_ROW_NUMBER,
        '{{ run_started_at }}' as INGESTION_TS_UTC
    FROM @{{ full_stage_name }}/
)
PATTERN = '{{ file_pattern }}'
{%- if full_format_name %}
, FILE_FORMAT = (FORMAT_NAME = '{{ full_format_name }}')
{%- endif %}
;
COMMIT;
{%- endmacro %}
```

不出所料，这与从 CSV 文件摄取的宏几乎相同，但它用提供的 `field_expressions` 替换了生成编号列的 for 循环（$1, … $n），我们在前面的代码中突出显示了这一点。

理论上，通过将 for 循环移到生成编号列（$1, … $n）的宏中并将其用作参数，你可以使用如下调用始终使用此模式：

```text
field_expressions = positional_columns(field_count)
```

我们更喜欢保留两种模式，以保持开发人员的流程简单。
