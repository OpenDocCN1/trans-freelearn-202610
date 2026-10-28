```
CALCULATETABLE(
    VALUES('Date'[Full Date])
    , 'Date'[IsValidDate] = TRUE()
)
, -1
, MONTH
)
```

Use the following DAX expressions to calculate Internet Sales YTD with Valid Dates:

```
Internet Sales YTD with Valid Dates =
CALCULATE(
    TOTALYTD(
        [Internet Sales]
    , CALCULATETABLE(
            VALUES('Date'[Full Date])
        , 'Date'[IsValidDate] = TRUE()
        )
    )
)
```

Use the following DAX expressions to calculate Internet Sales LYTD with Valid Dates:

```
Internet Sales LYTD with Valid Dates =
TOTALYTD (
    [Internet Sales]
    , DATEADD(
        CALCULATETABLE(
            VALUES('Date'[Full Date])
        , 'Date'[IsValidDate] = true()
        )
    , -1
    , YEAR
    )
)
```

And finally, use the following DAX expressions to calculate Internet Sales LY MTD with Valid Dates: