```
```sql
(select max({{high_watermark_column}}) from {{ this }})
{%- endif %}
)
```
```

Let’s analyze the code of this CTE in detail:

+   - The first three clauses of this CTE (`SELECT`, `FROM`, and `JOIN`) select all the columns from the input model and join it with the HDIFF column from the HIST table.
+   
+     The main thing to notice here is that only data from the input model is returned and that `JOIN` is a `LEFT OUTER JOIN`, potentially keeping all the rows from the input.
+   
+   - The rest of the CTE is all about the `WHERE` clause.

The first filter is the most important and the only one that’s needed:

```
```sql
WHERE h.{{diff_column}} is null
```
```

When the JOIN column on the HIST side is not null, this means that there was a match between an input HDIFF and a HIST HDIFF. This means that the two rows represent the same version and the HIST already has it.

When the JOIN column on the HIST side is null, it means that the row is either a new key or it is a different version of an old key and we need to store it.

The first part of the WHERE clause just keeps the rows that we want to store.

+   - The second filter allows us to unconditionally filter the incoming data:

```
```sql
and {{input_filter_expr}}
```
```

This filter allows us to remove data that we do not want to save in the history but cannot be filtered out directly in the input model.

+   - The third filter is used to apply the high watermark logic:

```
```sql
{%- if high_watermark_column %}
  and {{high_watermark_column}} {{high_watermark_test}}
    (select max({{high_watermark_column}})
     from {{ this }})
{%- endif %}
```
```

The if conditional ensures that the filter is added when a high watermark column name is passed since the code would not make sense without one.

The actual SQL code would generate a simple expression that filters the desired column based on the MAX() value found in the HIST table, as shown in the following example:

```
```sql
and LAST_UPDATED >=
    (select MAX(LAST_UPDATED) from HIST_TABLE)
```
```