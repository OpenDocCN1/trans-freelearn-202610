# Useful Placeholder Controls

With the DataGrid created, we are ready to insert columns. The first column that we will add is the Edit/Update column. In DataGrid speak, that is the EditCommandColumn. It contains an Edit link for authors to select a row in the grid that they wish to modify. When it is clicked, the row transforms into a series of textboxes. After authors have modified the contents of the row, they click the Update link to save their work back to the grid.

Of course, the Edit and Update links won't perform the work of editing and updating without our programming the necessary logic. We will do so by writing the OnEditCommand() method for editing and the OnUpdateCommand() method for updating, later on. Meanwhile, we register these methods to the DataGrid's EditCommand and UpdateCommand events respectively.

```
```csharp
// - - - code continues - - -
protected override void CreateAuthoringChildControls(
                                  BaseModeContainer authoringContainer)
{
    // Add a DataGrid control to the authoring container
    dg = new DataGrid();

    // Add an Edit/Update column to the DataGrid
    EditCommandColumn c0 = new EditCommandColumn();
    c0.EditText = "Edit";
    c0.UpdateText = "Update";
    dg.Columns.Add(c0);
    dg.EditCommand += new DataGridCommandEventHandler(this.OnEditCommand);
    dg.UpdateCommand += new DataGridCommandEventHandler(this.OnUpdateCommand);

    authoringContainer.Controls.Add(dg);
}
```
```

We will also provide a Delete button for authors to remove rows from the DataGrid. The column type that automatically creates a button in a DataGrid is the ButtonColumn. Like the edit and update links added earlier, we need to program the logic for deleting the items. We do so in the OnDeleteCommand() event handler, which we will work on later. For now, we will register the method to the DataGrid's DeleteCommand event as another DataGridCommandEventHandler.

```
```csharp
// - - - code continues - - -
protected override void CreateAuthoringChildControls(
                                  BaseModeContainer authoringContainer)
{
    // - - - code continues - - -

    // Add a Delete button to the DataGrid
    ButtonColumn c1 = new ButtonColumn();
    c1.CommandName = "Delete";
    c1.Text = "Delete";
    dg.Columns.Add(c1);
    dg.DeleteCommand += new DataGridCommandEventHandler(this.OnDeleteCommand);

    authoringContainer.Controls.Add(dg);
}
```
```

Next, we add a Label for error messages (if any) when loading and saving content from the DataGrid. Remember to define the Label as a class-wide variable so that it can be accessed by other methods within the class.

```
```csharp
// - - - code continues - - -
private Label lblMessage;
protected override void CreateAuthoringChildControls(
                                  BaseModeContainer authoringContainer)
```
```