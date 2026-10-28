most cases, we deal with data type conversion in table values. Either we use Power Query Editor UI or manually write the expressions; here, we use the following function:

```
Table.TransformColumnTypes(Table as table,
TypeTransformations as list, optional Culture as nullable text)
```

In the Table.TransformColumnTypes() function, we have the following:

+   - `Table` is usually the result of the previous step.
+   - `TypeTransformations` accepts a list of column names, along with their corresponding data type.
+   - `Culture` is optional and nullable text such as `en-NZ` specifying the culture.

We already discussed the types available in Power Query in Chapter 3, Data Preparation in Power Query Editor, in the Introduction to Power Query (M) Formula Language in Power BI section, in the Types subsection. The following table shows the types, along with the syntax we can use to specify the data types: