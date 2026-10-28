![](bbox=[0.175, 0.085, 0.825, 0.305])

# Figure 5.20: Result of adding a new custom column

As you can see, the new column’s data type is any, while we expect the output to be logical. So, here, we have two options. Most developers do the most trivial step by adding another Changed Type step to convert the new column into logical, which is not a good practice. What if we need to add some more custom columns? Will we add a Changed Type step after every new custom column? No, we do not need to add any extra steps after adding a new custom column. The second option is to use the ColumnType optional operand of the Table.AddColumn() function. The following expression shows the ColumnType optional operand within the Table.AddColumn() function:

```
Table.AddColumn(#"Merged Columns", "Is Yearly Income Below Average", each if [YearlyIncome] <= List.Average(#"Merged Columns"[YearlyIncome]) then true else false, type logical)
```

The output now looks as follows, without us adding any new steps: