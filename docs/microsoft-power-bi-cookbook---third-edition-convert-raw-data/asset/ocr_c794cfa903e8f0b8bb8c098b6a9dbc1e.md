7. In the Forecast query group, create a new blank query called PriorYearMonthlySales and disable loading the query:

```
let
    CurrentYear = Date.Year(CurrentDate()),
    CurrentMonth = Date.Month(CurrentDate()),
    WorkingMonth =
        if Date.DaysInMonth = Date.Day(CurrentDate())
            then CurrentMonth - 1 else CurrentMonth,
    PYJoin = Table.Join(
        #"Internet Sales","OrderDate",
        Dates,"FullDateAlternateKey",JoinKind.Inner),
    PYFilter = Table.SelectRows(PYJoin, each [CalendarYear] = CurrentYear-1 and
        [MonthNumberOfYear] >= WorkingMonth),
    PYGroup = Table.Group(
        PYFilter,{"CalendarYear", "MonthNumberOfYear", "EnglishMonthName"},
        {"Sales", each List.Sum([SalesAmount]), Currency.Type}),
    RemoveColumns = Table.RemoveColumns(PYGroup,{"MonthNumberOfYear"}),
    Month = Table.TransformColumns(
        RemoveColumns,
        {{"EnglishMonthName", each Text.Start(_, 3), type text}}),
    Rename = Table.RenameColumns(Month,{{"EnglishMonthName", "Month"}})
    in
    Rename
```

8. In the Forecast query group, create a new blank query called CurrentYearMonthlySales and disable loading the query:

```
let
    CurrentYear = Date.Year(CurrentDate()),
    CurrentMonth = Date.Month(CurrentDate()),
    WorkingMonth =
        if Date.DaysInMonth = Date.Day(CurrentDate())
            then CurrentMonth else CurrentMonth - 1,
    PYJoin = Table.Join(
        #"Internet Sales","OrderDate",
        Dates,"FullDateAlternateKey",JoinKind.Inner),
    PYFilter = Table.SelectRows(PYJoin, each [CalendarYear] = CurrentYear and
        [MonthNumberOfYear] <= WorkingMonth),
    PYGroup = Table.Group(
        PYFilter,{"CalendarYear", "MonthNumberOfYear", "EnglishMonthName"},
```