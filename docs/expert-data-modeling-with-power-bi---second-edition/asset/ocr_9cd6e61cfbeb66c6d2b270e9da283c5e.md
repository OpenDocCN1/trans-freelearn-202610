```
, 0, REL
, ORDERBY( DimDate[Year], ASC, DimDate[MonthOrder], ASC,
DimDate[Month])
, PARTITIONBY(DimDate[Year])
)
)
```

In the preceding code, we have to use DimDate[MonthOrder] first and then DimDate[Month] in the ORDERBY() function. The following image shows the results:

![](bbox=[0.177, 0.302, 0.822, 0.63])

Figure 15.49: Calculating Sales YTD using the WINDOW() function

So far, we have learned how the WINDOW() function works when the from_type parameter is ABS (absolute) and the to_type parameter is REL (relative) to calculate the running total on a single partition of data. Now let us take one step further and calculate a three-month moving average of sales. As the moving average must be calculated on a moving window over the data, both the from_type and to_type parameters must be REL (relative). Run the following measure:

```
Window, 3M Moving Avg =
AVERAGEX(
```