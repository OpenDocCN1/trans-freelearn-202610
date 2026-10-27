The following table provides an overview of the commands, functions, and operators we will be covering in the rest of this chapter:

| Type | Name | Description |
|---|---|---|
| Tabular Operators | print | Prints the results of a query |
|  | search | Searches for specific data throughout the logs |
|  | where | Filters a table to the subset of rows that satisfy a comparison |
|  | take/limit | Returns up to the specified number of rows |
|  | count () | Returns the number of records in the input record set |
|  | summarize | Produces a table that summarizes the content of the selected columns |
|  | extend | Creates calculated columns and appends them to the result set |
|  | project | Selects the columns to include |
|  | distinct | Produces a table with the distinct combination of the provided columns of the input table |
|  | sort/order | Sorts the rows of the input table into order by one or more columns |
|  | join | Merges the rows of two tables to form a new table by matching the values of the specified column(s) from each table |
|  | union | Takes two or more tables and returns the rows of all of them |
|  | render | Displays the output in a chart, including pie, bar, and line charts, among others |
| Query Statements | let | Creates variables |
| Scalar Functions | ago() | Subtracts the given timespan from the current UTC clock time |
| String Operators | operators | Not a command but a discussion of operators to use with the where command |