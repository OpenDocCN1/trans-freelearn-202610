+   - `blanks` is an optional parameter defining how to deal with the blank values while sorting. The only accepted value is currently `KEEP`, which is also the default value.
+   - We use the `PARTITIONBY()` function to define the optional `PartitionBy` parameter. If the `relation` parameter is specified, the columns used in the `PartitionBy` parameter must come from the `relation` table. If omitted, the `relation` parameter acts as a single partition.

Let us see how it works in action. We want to get the best-selling product. The following measure caters to that:

```
Index, Best Selling Product =
VAR __ProductSale =
    ADDCOLUMNS (
        SUMMARIZE (FactInternetSales
        , DimProduct[Product])
        , "ProductSales", [Sales]
    )
VAR __BestSeller =
    INDEX(1
        , __ProductSale
        , ORDERBY([ProductSales], DESC)
    )
RETURN
    CONCATENATEX(
        __BestSeller
        , 'DimProduct'[Product] & " : " &
        FORMAT([ProductSales], "$#,##.#")
    )
```

In the preceding code, we first defined a virtual table containing products’ sales (__ProductSale). Then we used it in the INDEX() function’s relation parameter, which is sorted in descending (DESC) order in the ORDERBY() function, and we asked for the first row of the results. Last but not least, we concatenated the values of the resulting columns. As you may have noticed, the preceding calculation has only one data partition.

The following image shows how the preceding code works when we run its different parts in DAX Studio: