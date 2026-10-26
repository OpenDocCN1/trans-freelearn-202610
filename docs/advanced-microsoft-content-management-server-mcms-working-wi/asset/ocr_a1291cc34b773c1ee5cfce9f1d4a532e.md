```
// It's the same as that of the existing posting
Channel currentChannel = p.Parent;

// Create a new posting based on the new template
if(currentChannel.CanCreatePostings)
{
    Posting newPosting = currentChannel.CreatePosting(t);

    // Get the name of the posting
    string postingName = p.Name;

    // Copy all properties that can be set, except for the name
    newPosting.DisplayName = p.DisplayName;
    newPosting.Description = p.Description;
    newPosting.ExpiryDate = p.ExpiryDate;
    newPosting.StartDate = p.StartDate;
    newPosting.IsHiddenModePublished = p.IsHiddenModePublished;
    newPosting.IsImportant = p.IsImportant;
    newPosting.IsRobotFollowable = p.IsRobotFollowable;
    newPosting.IsRobotIndexable = p.IsRobotIndexable;
    newPosting.SortOrdinal = p.SortOrdinal;

    // Copy all custom property values.
    CopyPostingCustomProperties(p, newPosting);

    // Copy all placeholder values
    CopyPostingContent(p, newPosting);

    // Commit all changes
    cmsContext.CommitAll();

    // Delete the existing posting
    if(p.CanDelete)
    {
        p.Delete();
        cmsContext.CommitAll();
    }

    // Set the posting's name
    newPosting.Name = postingName;
    cmsContext.CommitAll();

    // Display the posting that we have just created on the parent window
    // Close the dialog
    string script = "";
    string url = "";
    url = WebAuthorContext.Current.GetUrlForMode(newPosting, WebAuthorContextMode.PresentationUnpublished);
    script += "<script language=\"javascript\">";
    script += "window.opener.location.href=\"" + url + "\";";
    script += "window.close();";
    script += "</script>";
    if (!Page.IsClientScriptBlockRegistered("RedirectToNewPosting"))
    {
        Page.RegisterClientScriptBlock("RedirectToNewPosting",script);
    }
}
}
```