| StartItem Type | Options Added |
|---|---|
| Channel | New Channel
New Posting |
| TemplateGallery | New Template Gallery
New Template |
| ResourceGallery | New Resource |

For example, the options "New Channel" and "New Posting" will be added if the startItem is a channel and if the user has rights to create channels and postings within the parent channel.

Below ddlNewItem_SelectedIndexChanged() add the following PrepareToolbar() method, which inserts the options in the drop-down list:

```
private void PrepareToolbar()
{
    // remove any pre-existing options from the DropDownList
    ddlNewItem.Items.Clear();
    ddlNewItem.Items.Add(new ListItem("New",""));

    ListItem li = null;

    if (startItem is Channel)
    {
        Channel currentChannel = startItem as Channel;

        // if the user has rights to create channels, add option to create
        // a new channel
        if (currentChannel.CanCreateChannels)
        {
            li = new ListItem("New Channel","NewChannel");
            ddlNewItem.Items.Add(li);
        }

        // if the user has rights to create postings, add option to create
        // a new posting
        if (currentChannel.CanCreatePostings)
        {
            li = new ListItem("New Posting","NewPosting");
            ddlNewItem.Items.Add(li);
        }

        imgTitle.ImageUrl = "images/channelopen_big.gif";
    }
    else if (startItem is TemplateGallery)
    {
        TemplateGallery templateGallery = startItem as TemplateGallery;

        // if the user has rights to create template galleries, add option
        // to create a new template gallery
        if (templateGallery.CanCreateTemplateGalleries)
        {
            li = new ListItem("New Template Gallery",
                "NewTemplateGallery");
            ddlNewItem.Items.Add(li);
        }

        // if the user has rights to create templates, add option to create
        // a new template
        if (templateGallery.CanCreateTemplates)
```