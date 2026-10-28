the `PartitionBy` parameter is defined using the `PARTITIONBY()` function, the calculations reset for each partition. For instance, if we want to calculate the rolling average of sales over partitions of years, the equation resets the calculation at the beginning of each year.

# PARTITIONBY()

The values of specified columns in the `PARTITIONBY()` function are used to partition the table rows in the window functions. Its syntax is as follows:

```
PARTITIONBY ( <partitionBy_columnName>[,
partitionBy_columnName [, ...] ] )
```

For instance, we can imagine the `PARTITIONBY(DimDate[Year], DimDate[Month], DimDate[MonthOrder])` function resulting in creating partitions, as the following image shows: