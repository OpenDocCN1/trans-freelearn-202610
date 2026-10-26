# Useful Placeholder Controls

Finally, we add a Label control to the authoring container. The Label will be used to display error messages that may occur along the way. Like the TextBox, we will make it a class-wide variable as we will be using it within other methods.

```
private TextBox txtAuthoring;
private Label lblMessage;

protected override void CreateAuthoringChildControls
        (BaseModeContainer authoringContainer)
{
    - - - code continues - - -

    // Add a label for displaying messages
    lblMessage = new Label();
    authoringContainer.Controls.Add(lblMessage);
}
```

# Retrieving Saved Content

When the posting is opened for editing, the textbox of the date time picker control shows the previously saved date and time value. For new postings, there won't be a previous selection; we will just use the current date and time. Override LoadPlaceholderContentForAuthoring():

```
protected override void LoadPlaceholderContentForAuthoring
        (PlaceholderControlEventArgs e)
{
    EnsureChildControls();
    try
    {
        if (WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringNew)
        {
            // The default value of the control is the current date and time
            txtAuthoring.Text = DateTime.Now.ToString("dd MMM yyyy HH:mm");
        }
        else
        {
            txtAuthoring.Text = ((HtmlPlaceholder)this.BoundPlaceholder).Text;
        }
    }
    catch(Exception ex)
    {
        lblMessage.Text = "Failed to load placeholder content: " + ex.Message;
    }
}
```

# Saving the Date and Time

When the posting is saved, we read the value stored in the TextBox and write it to the underlying HtmlPlaceholder. Any exceptions are trapped using a try-catch block and the detailed error message appears on screen. Add the overridden SavePlaceholderContent() method to the code:

```
protected override void SavePlaceholderContent(PlaceholderControlSaveEventArgs e)
{
    EnsureChildControls();
    try
    {
        ((HtmlPlaceholder)this.BoundPlaceholder).Html = txtAuthoring.Text;
    }
    catch(Exception ex)
    {
```