```
//fnODataFeedAnalysis
(ODataFeed as text) as table =>
let
    Source = OData.Feed(ODataFeed),
    FilterTables = Table.SelectRows(Source, each Type.Is(Value.Type([Data]), Table.Type) = true),
    #"TableColumnCount Added" = Table.AddColumn(FilterTables, "Table Column Count", each Table.ColumnCount([Data]), Int64.Type),
    #"TableCountRows Added" = Table.AddColumn(#"TableColumnCount Added", "Table Row Count", each Table.RowCount([Data]), Int64.Type),
    #"NumberOfDecimalColumns Added" = Table.AddColumn(#"TableCountRows Added", "Number of Decimal Columns", each List.Count(Table.ColumnsOfType([Data], {Decimal.Type})), Int64.Type),
    #"ListOfDecimalColumns Added" = Table.AddColumn(#"NumberOfDecimalColumns Added", "List of Decimal Columns", each if [Number of Decimal Columns] = 0 then null else Table.ColumnsOfType([Data], {Decimal.Type})),
    #"NumberOfTextColumns Added" = Table.AddColumn(#"ListOfDecimalColumns Added", "Number of Text Columns", each List.Count(Table.ColumnsOfType([Data], {Text.Type})), Int64.Type),
    #"ListOfTextColumns Added" = Table.AddColumn(#"NumberOfTextColumns Added", "List of Text Columns", each if [Number of Text Columns] = 0 then null else Table.ColumnsOfType([Data], {Text.Type})),
    #"Sorted Rows" = Table.Sort(#"ListOfTextColumns Added", {{"Table Column Count", Order.Descending}, {"Table Row Count", Order.Descending}}),
    #"Removed Other Columns" = Table.SelectColumns(#"Sorted Rows", {"Name", "Table Column Count", "Table Row Count", "Number of Decimal Columns", "List of Decimal Columns", "Number of Text Columns", "List of Text Columns"})
in
    #"Removed Other Columns"
```

To invoke the preceding custom function, we need to pass the OData URL to the preceding function, and it gives us a result set as follows: