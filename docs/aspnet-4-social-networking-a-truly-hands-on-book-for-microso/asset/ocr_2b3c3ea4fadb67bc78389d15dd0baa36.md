# Messaging

Loading messages into the UI is simply a matter of binding the DataSource to the repeater.

```
public void LoadMessages(List<PMessage> Messages)
{
    repMessages.DataSource = Messages;
    repMessages.DataBind();
}
```

As this requires no further explanation, let's jump right into how we go about building our page navigation. Recall that in the Init() of our presenter we had a call into _view.DisplayPageNavigation(), which received the PageCount of the folder we were working with, and the current page we were viewing. Here is that method:

```
public void DisplayPageNavigation(Int32 PageCount, MessageFolders folder, Int32 CurrentPage)
{
    if(PageCount == CurrentPage)
        linkNext.Visible = false;
    if (CurrentPage == 1)
        linkPrevious.Visible = false;
    linkNext.NavigateUrl = "~/mail/default.aspx?folder=" + ((int)folder).ToString() + "&page=" + (CurrentPage + 1).ToString();
    linkPrevious.NavigateUrl = "~/mail/default.aspx?folder=" + ((int)folder).ToString() + "&page=" + (CurrentPage - 1).ToString();
    for(int i = 1; i<=PageCount;i++)
    {
        HyperLink link = new HyperLink();
        link.Text = i.ToString();
        link.NavigateUrl = "~/mail/default.aspx?folder=" + ((int)folder).ToString() + "&page=" + i.ToString();
        phPages.Controls.Add(link);
        phPages.Controls.Add(new LiteralControl("&nbsp;"));
    }
}
```

This chunk of code interacts with three controls in our UI—two hyperlinks, one that displays **Previous** and one displaying **Next**, and a **PlaceHolder** control that will hold the individual page numbers of all the pages for this data set.