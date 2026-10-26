```
// Text
tableCell = new TableCell();
tableRow.Cells.Add(tableCell);
ltCn = new LiteralControl();
ltCn.Text = "Render Author";
tableCell.Controls.Add(ltCn);

tableCell = new TableCell();
tableCell.Width = 10;
tableRow.Cells.Add(tableCell);

// Date and Author
tableCell = new TableCell();
tableRow.Cells.Add(tableCell);
chkShowAuthor = new CheckBox();
chkShowAuthor.EnableViewState = false;
chkShowAuthor.ID = "RenderAuthor";
chkShowAuthor.Checked = true;
tableCell.Controls.Add(chkShowAuthor);
```

Then we add a placeholder called baseAuthoringContainer, and add the outer table to it. The baseAuthoringContainer is then added to the controls collection of the authoring container:

```
// Finally the Authoring Container
this.baseAuthoringContainer = new System.Web.UI.WebControls.PlaceHolder();
baseAuthoringContainer.Controls.Add(OuterTable);
baseAuthoringContainer.ID = "RssPhAuthoringControl";
authorContainer.Controls.Add(this.baseAuthoringContainer);
}
```

To load the value from the XmlPlaceholder string, we'll create a method called LoadSettingsFromXml(). The contents of the XmlPlaceholder will be loaded into an XmlDocument and then the variables we previously defined will be populated.

```
/// <summary>
/// Load the contents of the placeholder for
/// authoring mode
/// </summary>
/// <param name="e"></param>
protected override void LoadPlaceholderContentForAuthoring(PlaceholderControlEventArgs e)
{
    try
    {
        // check the child controls were created successfully
        EnsureChildControls();

        // Load the settings for the placeholder control
        this.LoadSettingsFromXml(this.BoundXmlPlaceholder.XmlAsString);

        // Set the values of the Web controls
        this.txtRSSUrl.Text = this.RssUrl;
        this.ddlItemsToDisplay.SelectedItem.Value = this.ItemsToDisplay.ToString();
        this.chkShowOpenArticleLink.Checked = this.ShowOpenArticleLink;
        this.chkShowDescription.Checked = this.ShowDescription;
        this.chkShowDate.Checked = this.ShowDate;
        this.chkShowAuthor.Checked = this.ShowAuthor;
    }
    catch(Exception ex)
    {
        // Handle any exceptions
    }
}
```