## 从 CSV 文件加载

现在我们已经设置了摄取数据集所需的<|message|>
```jinja
{% macro load_<table_name>(
    db_name = get_XXX_ingestion_db_name(),
    schema_name = get_XXX_ingestion_schema_name(),
    stage_name = get_XXX_ingestion_stage_name(),
    format_name = None,
    table_name = 'YYYYYY',
    pattern = '.*YYYYYY*[.]csv',
    field_count = 12
) %}
{% set full_table_name =
        db_name ~ '.' ~ schema_name ~ '.' ~ table_name %}
{% do run_query(X_Y__create_table_sql(full_table_name)) %}
{% do run_query(ingest_into_landing_sql(
        full_table_name,
        field_count = field_count,
        file_pattern = pattern,
        full_stage_name = stage_name,
        full_format_name = format_name
    )) %}
{%- endmacro %}
```

首先要注意的是，我们将主宏命名为 `load`，加上要摄取的表名。

然后，我们使用参数声明将它们设置为使用设置文件中定义的 getter 或通过提供特定于此表的值。

它们是表的名称，将是落地表的名称，用于从阶段加载文件的模式，以及 CSV 文件中的列数。

请注意，我们在这里不需要列名，因为我们在 `CREATE TABLE` 宏中定义它们。

要选择要加载的文件，你可以提供一个简单的正则表达式；Snowflake 只会加载尚未在 `COPY INTO` 命令引用的表中加载的文件，或自加载以来被修改过的文件。

然后，我们必须构建落地表的完全限定名称，并使用它调用创建表特定宏，将结果传递给 `run_query()` 函数以执行生成的 SQL 代码。

请注意，出于简洁考虑，我们已从宏中删除了日志记录指令。

最后一次操作通过传递在主宏声明中为方便使用而设置的参数来调用 `ingest_into_landing_sql` 库宏。

创建表宏非常简单，并且做其名称所暗示的事情：

```sql
{% macro X_Y__create_table_sql(full_table_name) %}
CREATE TRANSIENT TABLE IF NOT EXISTS {{ full_table_name }}
(
        -- Add declaration for table columns
        -- column type [[NOT] NULL],
        YYY_CODE string NOT NULL,
        YYY_NAME string NULL,

        -- metadata
        FROM_FILE string,
        FILE_ROW_NUMBER integer,
```




```sql
INGESTION_TS_UTC TIMESTAMP_NTZ(9)
)
COMMENT = '...';
{%- endmacro %}
```

如你所见，这里没什么好说的，因为该宏只是封装了一个普通的 CREATE TABLE SQL 命令。

有趣的一点是，你应该在表的初始部分声明 CSV 中存在的所有列，将三个元数据字段放在最后，因为它们是由摄取宏在最后三个位置自动创建的。

作为说明，你可以定义字段的可空性，默认是可空的。除非你想要一个非常严格且不灵活的加载设置，我们建议将所有列都设为可空，并通过你在读取落地表的源上定义的测试来检查你希望设为非空的列。

最后，让我们来看看 `ingest_into_landing_sql` 库宏的实现，它实现了 COPY INTO 命令的调用：

```sql
{% macro ingest_into_landing_sql(
    full_table_name,
    field_count,
    file_pattern,
    full_stage_name,
    full_format_name
) %}
BEGIN TRANSACTION;
COPY INTO {{ full_table_name }}
FROM (
    SELECT
    {% for i in range(1, field_count+1) %}${{i}},{% endfor %}

    METADATA$FILENAME as FROM_FILE,
    METADATA$FILE_ROW_NUMBER as FILE_ROW_NUMBER,
    '{{ run_started_at }}' as INGESTION_TS_UTC
    FROM @{{ full_stage_name }}/
)
PATTERN = '{{ file_pattern }}'
{%- if full_format_name %}
FILE_FORMAT = (FORMAT_NAME = '{{ full_format_name }}')
{%- endif %}
;
COMMIT;
{%- endmacro %}
```

如你所见，摄取本身非常简单——它涉及在事务中调用 COPY INTO 命令并传递落地表、stage、文件模式以及最终用于覆盖 stage 的文件格式的正确名称。

CSV 摄取的宏会生成 $1、$2... 位置列名来加载 CSV 的负载，然后添加元数据列：文件名、文件中的行号以及加载时间（我们开始 dbt 运行的时间）。
