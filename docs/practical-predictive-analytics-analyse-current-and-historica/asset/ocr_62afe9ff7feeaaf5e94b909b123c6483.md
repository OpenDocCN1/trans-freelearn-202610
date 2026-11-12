# Creating some global views

Creating global views will also allow us to pass data between different databricks notebooks. These views will be referenced in the next section. Use the `%sql` magic command as the first line in the databricks notebook to signify that these are SQL statements:

```
%sql
CREATE GLOBAL TEMPORARY VIEW df_view AS SELECT * FROM df
```

```
%sql
CREATE GLOBAL TEMPORARY VIEW test_view AS SELECT * FROM test
```

```
%sql
CREATE GLOBAL TEMPORARY VIEW out_sd_view AS SELECT * FROM out_sd
```

```
%sql
CREATE GLOBAL TEMPORARY VIEW sumdf_view AS SELECT * FROM sumdf
```

# User exercise

After the views have been created, use SQL to read back the counts and verify the totals with the row counts produced for the original dataframes:

```
%sql
select count(*) from global_temp.df_view union all
select count(*) from global_temp.test_view union all
select count(*) from global_temp.sumdf_view union all
select count(*) from global_temp.out_sd_view
```

| count(1) |
|---|
| 12380000 |
| 298000 |
| 5 |
| 1536000 |