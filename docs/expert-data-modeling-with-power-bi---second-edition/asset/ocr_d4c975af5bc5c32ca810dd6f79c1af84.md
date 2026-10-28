```
Internet Sales LY MTD =
TOTALMTD(
    [Internet Sales]
    , SAMEPERIODLASTYEAR('Date'[Full Date])
)
```

Note

The SAMEPERIODLASTYEAR('Date'[Full Date]) and DATEADD('Date'[Full Date], -1, YEAR) act the same. We used different functions to demonstrate the possibilities.

We can then put a table visual on the report canvas to show the calculations side by side with **Full Date** from the **Date** table. Everything looks good unless we sort the **Full Date** column in descending order, which is when we realize that there is an issue. We get null values for the **Internet Sales**, **Internet Sales MTD**, and **Internet Sales LMTD** measures for many dates. We are also getting a lot of duplicate values for the **Internet Sales YTD** measure.

The following figure illustrates the results: