This will show us the temporary view that we just created, as illustrated in the following screenshot:

![](bbox=[0.11, 0.18, 0.85, 0.42])

Figure 8.2 - Using SQL to read from a CSV file

If we read a CSV file specifying the schema, we may encounter some issues if the specified schema differs from what we have specified—for example, we may have a string value in a column that was specified as an integer data type. This could lead to results that differ considerably from the actual data. Therefore, it is always good practice to verify the correctness of the data.

We can specify the behavior of the CSV parser by selecting one of the available modes in which the parser runs. The available options for this mode are outlined here:

+   - **PERMISSIVE**: This is the default mode. Here, null values are inserted in fields that have not been parsed correctly. This allows us to inspect rows that have not been parsed correctly.
+   - **DROPMALFORMED**: This mode drops lines that hold values that could not be parsed correctly.
+   - **FAILFAST**: This mode aborts the reading of the file if any malformed data is found.

To set the mode, use the mode option, as illustrated in the following code snippet:

```
drop_wrong_schema = sqlContext.read.format("csv").
option("mode", "FAILFAST")
```