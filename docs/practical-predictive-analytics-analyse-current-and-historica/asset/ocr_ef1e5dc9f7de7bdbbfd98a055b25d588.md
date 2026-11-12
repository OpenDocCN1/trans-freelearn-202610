# Issuing SQL through the R interface

Writing SQL this way consists of constructing a valid SQL string query via the `sql` function:

+   - The query must reference a registered table

+   - You can assign the results of the query to a dataframe:

Here is an example of getting a frequency count of the available samples from the registered object out_table. Â Note that the code begins with %r command which is a special Databricks directive that indicates that the code that follows applies to R. Â However, it is sometimes necessary to specify this (even in R notebooks), when a previous Â code chunks has used another language directive, such as %sql, or %python.

```
%r
rm(tmp)
tmp <- SparkR::sql(sqlContext, "SELECT sample_bin, count(*) FROM out_tbl group by sample_bin")
```

+   - Additionally in databricks, sqlcontext is preassigned, and so can be omitted in the code as follows.

```
tmp <- SparkR::sql("SELECT sample_bin , count(*) FROM out by sample_bin")
```

+   - Once you have extracted the data to a tmp dataframe, you can display the counts of each of the samples. In subsequent queries, you can select samples corresponding to the size and proportion of the outcome you are interested in: