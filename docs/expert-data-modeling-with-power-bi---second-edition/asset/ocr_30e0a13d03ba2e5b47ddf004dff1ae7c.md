```
Internet Sales LY MTD with Valid Dates =
TOTALMTD(
    [Internet Sales]
    , SAMEPERIODLASTYEAR(
        CALCULATETABLE(
            VALUES('Date'[Full Date])
            , 'Date'[IsValidDate] = TRUE()
        )
    )
)
```

The following figure shows the results of putting the preceding measures in a table visual without filtering the visual with the IsValidDate column:

![](bbox=[0.177, 0.35, 0.825, 0.58])

Figure 2.26: Periodic calculations with improved measures

This method also fixes the issue of showing invalid dates when we use the Full Date column in a slicer visual. It is much nicer if we only show the valid dates in the slicer. The following figure illustrates a slicer visual showing Full Range Dates ranges from the Date table, side by side with another slicer visual showing Valid Dates Only: