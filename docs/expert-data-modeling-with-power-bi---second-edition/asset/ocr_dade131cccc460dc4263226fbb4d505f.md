In this subsection, we looked at a simple example of using the `OFFSET()` function. Microsoft’s official documentation can be found here: [https://learn.microsoft.com/en-us/dax/offset-function-dax?WT.mc_id=DP-MVP-5003466](https://learn.microsoft.com/en-us/dax/offset-function-dax?WT.mc_id=DP-MVP-5003466).

The `WINDOW()` function comes next.

# WINDOW()

The `WINDOW()` function retrieves multiple rows calculated based on the given position range. The range is calculated based on the mandatory parameters: `from`, `from_type`, `to`, and `to_type`. The `from` and `to` positions can be either `ABS` (absolute) or `REL` (relative), which are defined by the `from_type` and `to_type` parameters. The `from` parameter, depending on the `from_type`, indicates the first row from the current row, and the `to` parameter indicates the last row based on the `to_type` parameter.

The syntax of this function is:

```
WINDOW ( from[, from_type], to[, to_type][, <relation>][, <orderBy>][, <blanks>][, <partitionBy>])
```

All parameters other than the first four are similar to the `INDEX()` parameters in the preceding syntax. The `WINDOW()` function can be used to solve some scenarios that would be complex to overcome without the `WINDOW()` function, such as calculating moving averages. For instance, the following measure calculates the running total of sales by `Product Category` and `Product Subcategory`:

```
Window, RT Sales by Product Category and Subcategory =
CALCULATE(
    [Sales]
    , WINDOW(
          1, ABS
        , 0, REL
        , ORDERBY(DimProduct[Product Category], ASC,
DimProduct[Product Subcategory], ASC)
    )
)
```