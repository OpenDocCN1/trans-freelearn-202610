6. Create a new blank query called Dates that uses the following code to add a surrogate sequential column to be used in date intelligence measures:

```
let
    Source = BaseDateTable,
    YearMonthCols =
        Table.Distinct(
            Table.SelectColumns(
                Source, {"Calendar Year","Calendar Month Number"}
            )
        ),
    YearMonthColSort =
        Table.Sort(
            YearMonthCols,
            {
                {"Calendar Year", Order.Ascending},
                {"Calendar Month Number", Order.Ascending}
            }
        ),
    YearMonthColIndex =
        Table.AddIndexColumn(YearMonthColSort, "YearMonthIndex",1,1),
    JoinedDateTable =
        Table.NestedJoin(
            DatesInterim, {"Year", "Month Number"},
            YearMonthColIndex, {"Calendar Year","Calendar Month Number"},
            "Year-Mo Index", JoinKind.Inner
        ),
    IndexColumnAdded =
        Table.ExpandTableColumn(
            JoinedDateTable,
            "Year-Mo Index",{"YearMonthIndex"},{"Year Month Number"}
        )
in
    IndexColumnAdded
```

7. Disable loading on BaseDateTable and DatesInterim by right-clicking the queries and deselecting Enable load. Only the Dates query needs to be loaded into the semantic model.

# How it works

For the DatesInterim query (steps 4 and 5), the CurrentDate, CurrentMonth, and CurrentYear expressions could be stored as separate queries. The DateTime.LocalNow function is used in dynamic M date logic, similar to the CURRENT_TIMESTAMP function used in SQL statements.