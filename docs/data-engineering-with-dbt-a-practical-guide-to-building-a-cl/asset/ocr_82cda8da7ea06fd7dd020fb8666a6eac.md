```
high_watermark_test = '>=',
order_by_expr = none
) -%}
```

Let's analyze them one at a time:

+   - The `input_rel` parameter is an object of the `Relation` type, as returned by the `ref()` or `source()` functions, that provides the input data to be saved.
+   
+     In our PDP architecture, this is generally an STG model, which, in turn, is reading from a source, but any valid relation would work.
+   - The `key_column` parameter is a string with the name of the column to be used as a key to determine the most recent version of an instance.
+   
+     This parameter is essential for reading data out of the HIST table as it allows us to identify instances of the entity stored in the table, and therefore select the most recent version – that is, the “current” version.
+   - The `diff_column` parameter is a string that specifies the name of the column that holds the HASH DIFF column. This can be used to compare different versions of the entities stored in the table. It must include the entity key.
+   - The `load_ts_column` parameter is an optional string with at least the name of the column to be used to order the data to understand what's current.
+   
+     By default, its value is `LOAD_TS_UTC` and points to the field that contains the time when the data was added to the data platform. It can be any column that increases when a new, more recent, version is added.
+   - The `input_filter_expr` parameter is an optional string with a SQL expression to be used to filter the input data.
+   
+     By default, its value is TRUE, which means no filtering of the input. This filter is applied both in incremental and non-incremental runs.
+   
+     For efficient operations, filtering is applied to the input model, but the parameter also allows us to filter using data coming from the HIST table itself.
+   
+     Another use case is when multiple models depend on the same input model, which means the filtering cannot be pushed back to the input model, as in the case of real-time Lambda architectures.
+   - The `history_filter_expr` parameter is an optional string with a SQL expression to be used to filter the HIST data.
+   
+     By default, its value is TRUE, which means no filtering of the HIST table. This filter is only applied in incremental runs since it applies to the HIST table.
+   
+     This parameter is seldom used as, normally, you want to compare the input data with the current version for all the contents of the HIST table, even if it's very old.