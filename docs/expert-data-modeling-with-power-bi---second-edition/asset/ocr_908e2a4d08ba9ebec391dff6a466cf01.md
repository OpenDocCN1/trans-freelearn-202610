```
, [__Sales]
)
, "__Result")
RETURN
    IF(HASONEVALUE(DimDate[Year]), __Result, BLANK())
```

The following image shows the results in the DAX Debug Output tool after using the Index, % of Best Selling Month EVALUATEANDLOG measure in a table visual:

![](bbox=[0.175, 0.3, 0.825, 0.695])

Figure 15.44: Evaluating the Index, % of Best Selling Month EVALUATEANDLOG measure in the DAX Debug Output tool

The numbers in the preceding image show:

+   1. The results of the __YearMonthSales variable.
+   2. The results of the __BestSellingMonthSales variable.
+   3. The results of the __Result variable.