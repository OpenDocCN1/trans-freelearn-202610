as an argument to the `filter()` function:

```
emp_ds.sort(col("empId").asc()).filter(col("salary").gt("2500"));
```

+   • groupBy(): This groups the dataset using the specified columns, so that one can run aggregation on them:

```
emp_ds.select("job").groupBy(col("job")).count().show();
```

+   • join(): This joins with another dataframe using the given join expression. The function accepts three parameters: the first being another dataset; the second being the column on which the data will be joined; and the third being joinType--inner, outer, left_outer, right_outer, and leftsemi. The following performs a left-outer join between emp_ds and deptDf:

```
emp_ds.as("A").join(deptDf.as("B"),emp_ds.col("deptno").equalTo(deptDf.col('
```