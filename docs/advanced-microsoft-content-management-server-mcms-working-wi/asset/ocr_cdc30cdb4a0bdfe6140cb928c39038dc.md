Next, we define each row and create two table cells. We add the label and the textbox web control to the table cells. This process is repeated for each setting the user can specify:

```
// ROW: Text
tableCell = new TableCell();
tableCell.Width = 130;
tableRow.Cells.Add(tableCell);
LiteralControl ltCn = new LiteralControl();
ltCn.Text = "RSS-Feed Url";
tableCell.Controls.Add(ltCn);

tableCell = new TableCell();
tableCell.Width = 10;
tableRow.Cells.Add(tableCell);

// ROW: Url Input Box
tableCell = new TableCell();
tableRow.Cells.Add(tableCell);
txtRSSUrl = new TextBox();
txtRSSUrl.ID = "RssUrl";
txtRSSUrl.Width = 300;
txtRSSUrl.TextMode = System.Web.UI.WebControls.TextBoxMode.SingleLine;
tableCell.Controls.Add(txtRSSUrl);

// New Row
tableRow = new TableRow();
TableHoldingTheControls.Rows.Add(tableRow);

// ROW: Number of Articles
tableCell = new TableCell();
tableRow.Cells.Add(tableCell);
ltCn = new LiteralControl();
ltCn.Text = "Number of Articles";
tableCell.Controls.Add(ltCn);

tableCell = new TableCell();
tableCell.Width = 10;
tableRow.Cells.Add(tableCell);

// Count Input Box
tableCell = new TableCell();
tableRow.Cells.Add(tableCell);
ddlItemsToDisplay = new DropDownList();
ddlItemsToDisplay.EnableViewState = false;
ddlItemsToDisplay.ID = "Count";
ddlItemsToDisplay.Items.Add("5");
ddlItemsToDisplay.Items.Add("10");
ddlItemsToDisplay.Items.Add("15");
ddlItemsToDisplay.Items.Add("20");
ddlItemsToDisplay.Items.Add(new ListItem("unlimited", int.MaxValue.ToString()));

ddlItemsToDisplay.SelectedIndex = 4;
tableCell.Controls.Add(ddlItemsToDisplay);

// New Row
tableRow = new TableRow();
TableHoldingTheControls.Rows.Add(tableRow);

// Text
tableCell = new TableCell();
tableRow.Cells.Add(tableCell);
ltCn = new LiteralControl();
ltCn.Text = "Render \"Open Article\" link";
```