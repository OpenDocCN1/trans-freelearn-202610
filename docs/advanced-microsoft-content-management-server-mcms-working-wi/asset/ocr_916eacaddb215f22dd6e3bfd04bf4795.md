5. In the Page_Load() event handler, we get an instance of the template to be moved. In previous examples, we did a check to see if the user has rights to work with an item only after the user has clicked on the button. Let's try a different approach this time. Instead of returning an error message to users who do not have the necessary permissions, we will enable or disable the button accordingly.
We check to see if the user has rights to move the template by checking the Template.CanMove property value. If Template.CanMove returns true, it means that the user is either an administrator or assigned to a rights group that belongs to a channel manager or template designer role of the current template gallery. If the user has the rights to move the template, we keep the btnMove button enabled; otherwise, it's disabled.

```
private CmsHttpContext cmsContext;
private Template originalTemplate;

private void Page_Load(object sender, System.EventArgs e)
{
    cmsContext = CmsHttpContext.Current;

    // Clear all error messages, if any.
    lblErrorMessage.Text = String.Empty;

    // Get an instance of the original template
    string cmsObjectGuid = String.Empty;
    if (Request.QueryString["CMSObjectGuid"] != null)
    {
        cmsObjectGuid = Request.QueryString["CMSObjectGuid"];
        originalTemplate = cmsContext.Searches.GetByGuid(cmsObjectGuid)
            as Template;

        if (originalTemplate != null)
        {
            // Display the template's path
            litOriginalTemplate.Text = originalTemplate.Path;

            if (originalTemplate.CanMove)
            {
                btnMove.Enabled = true;
            }
            else
            {
                // we can't move this template.
                lblErrorMessage.Text = "You do not have rights to " +
                    "move this template.";
                btnMove.Enabled = false;
            }
        }
        else
        {
            // Uh-oh we can't get an instance of the template.
            lblErrorMessage.Text = "Original Template not found!";
        }
    }
}
```

6. Next, we get an instance of the destination template gallery. We perform the move operation only if the user has rights to create templates in the destination template gallery. Once we have ascertained that the destination template gallery is valid, we will call the Template.MoveTo() method, passing the instance of the destination template gallery as an input parameter. After moving a template, its state is Saved.