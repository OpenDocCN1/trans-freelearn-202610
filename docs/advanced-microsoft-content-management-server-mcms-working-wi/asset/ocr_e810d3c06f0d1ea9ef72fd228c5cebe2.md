```
```csharp
{
    // Add a new row
    TableRow tr = new TableRow();
    table.Rows.Add(tr);
}
```
```

Each table row will consist of two columns. In the first, we will display the name of the source placeholder.

```
```csharp
private void DisplayListOfPHDefinitions(Template source, Template destination)
{
    - - - code continues - - -

    // Iterate through all placeholder definitions on the source template
    foreach(PlaceholderDefinition sourceDef in source.PlaceholderDefinitions)
    {
        - - - code continues - - -

        // Display the name of the source placeholder definition
        TableCell td1 = new TableCell();
        td1.Text = sourceDef.Name;
        tr.Cells.Add(td1);
    }
}
```
```

In the second column of each row, we will provide a dropdown. Authors will choose the destination placeholder from the items in this list.

```
```csharp
private void DisplayListOfPHDefinitions(Template source, Template destination)
{
    - - - code continues - - -

    // Iterate through all placeholder definitions on the source template
    foreach(PlaceholderDefinition sourceDef in source.PlaceholderDefinitions)
    {
        - - - code continues - - -

        // Generate a DropDownList of destination placeholder definitions
        DropDownList ddl = new DropDownList();
        ddl.ID = "ddl" + sourceDef.Name;

        // Add the dropdown list to a table cell
        TableCell td2 = new TableCell();
        td2.Controls.Add(ddl);
        tr.Cells.Add(td2);
    }
}
```
```

The dropdown will list the names of all placeholders in the new template. In addition, it will only show the placeholder if the placeholder definition is identical to that of the source template definition. Moving content between incompatible placeholder types (say from an HtmlPlaceholder to an ImagePlaceholder) does not make sense.

We will also add an option with a value of "None" to the dropdown. Authors can decide not to move content from selected source placeholders by selecting this option from the list.

```
```csharp
private void DisplayListOfPHDefinitions(Template source, Template destination)
{
    - - - code continues - - -

    // Iterate through all placeholder definitions on the source template
    foreach(PlaceholderDefinition sourceDef in source.PlaceholderDefinitions)
```
```