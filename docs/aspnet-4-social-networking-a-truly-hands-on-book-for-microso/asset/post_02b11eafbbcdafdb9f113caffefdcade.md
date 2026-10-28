
## Messaging

The Translator - AccountsToUserNames is taking in the accounts collection and returning a string of usernames of those accounts, which is what is finally bound to the repeater in the UI.

In the code behind for our friend control, we insert a snippet of JavaScript to allow us to click on a friend and carry his/her username into our To field in our NewMessage UI.

```csharp
//Fisharoo/Web/Mail/UserControls/Friends.aspx.cs
public void repFriends_ItemDataBound(object sender,
                                     RepeaterItemEventArgs e)
{
    if(e.Item.ItemType == ListItemType.Item || e.Item.ItemType ==
                                  ListItemType.AlternatingItem)
    {
        HyperLink linkFriend = e.Item.FindControl("linkFriend") as
                                      HyperLink;
        linkFriend.Attributes.Add("OnClick",
                          "javascript:document.forms[0].Content_txtTo.value += '' + e.Item.DataItem + ';';");
    }
}
```

Note that we are adding an attribute for the OnClick event of our link that will call a JavaScript to move the username to our To field with a semicolon delimiter.

This gives us a list of friends, but how do we tie that back into our UI? Open the NewMessage.aspx page. We will need to register the new control in the page and then add a reference to it in the page.

Just below the Page directive add the following code:

```
<%@ Register Src="~/Mail/UserControls/Friends.ascx"
             TagPrefix="Fisharoo" TagName="Friends" %>
```

Then add a new Content section to our page where the friends control will live:

```
<asp:Content ContentPlaceHolderID="LeftNavTop" runat="server">
    <Fisharoo:Friends ID="friends1" runat="server" />
</asp:Content>
```

This inserts the friends control into our left navigation bar as defined in the master page!

Loading messages into the UI is simply a matter of binding the DataSource to the repeater.

```csharp
public void LoadMessages(List<PMessage> Messages)
{
    repMessages.DataSource = Messages;
    repMessages.DataBind();
}
```

As this requires no further explanation, let's jump right into how we go about building our page navigation. Recall that in the Init() of our presenter we had a call into _view.DisplayPageNavigation(), which received the PageCount of the folder we were working with, and the current page we were viewing. Here is that method:

```csharp
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

This method initially determines if we should show the **Next** or **Previous** links based on our current page and our total page count. We then hook up the navigation property of each of those links to take us to the next page or the previous page of data. After that we use a for loop to iterate through all the possible pages, from 1 to PageCount, making a new hyperlink for each iteration that contains the location of that page.

The other feature of this page is the ability to delete messages—as many or as few as we like. This is primarily achieved with a helper function in the code behind the view that extracts all the messages that are selected, that can be called from within the presenter. This allows the presenter to remain in control!

```csharp
public List<Int32> ExtractSelectedMessages()
{
    List<Int32> result = new List<Int32>();
    foreach (RepeaterItem item in repMessages.Items)
    {
        if(item.ItemType == ListItemType.Item || item.ItemType ==
           ListItemType.AlternatingItem)
        {
            CheckBox chkMessage = item.FindControl("chkMessage") as
                                     CheckBox;
            Int32 messageID =
                Convert.ToInt32(chkMessage.Attributes["MessageID"]);
            if(chkMessage.Checked)
                result.Add(messageID);
        }
    }
    return result;
}
```

This method iterates through all the check boxes to see if they are selected or not. If they are, then it extracts the MessageID from an attribute that was created when we loaded the display. A collection of MessageIDs is then returned to the caller.

This then brings us to the **Delete** method in the presenter. It is called from a button click event that is bubbled up to the presenter, which then calls into the view to get a list of selected MessageIDs. Then using the MessageService we get a copy of that message. With the copy, we then call into the MessageRecipientService.DeleteMessageRecipient() method and delete each selected message.

```csharp
linkFolder.NavigateUrl = "~/Mail/Default.aspx?folder=" +
    ((MessageFolder)e.Item.DataItem).MessageFolderID.ToString();
linkFolder.Attributes.Add("FolderID",
    ((MessageFolder)e.Item.DataItem).MessageFolderID.ToString());
}
}
```

This creates a list of folders for us in a control. But how do we get it into our UI? To do this, we have to go back to our Default.aspx page. This will be done exactly the same way we did for our friends control in our new message page. Open the default page so that we can add the Folders control.

```
<%@ Register Src="~/Mail/UserControls/Folders.ascx"
    TagPrefix="Fisharoo" TagName="Folders" %>
<asp:Content ContentPlaceHolderID="LeftNavTop" runat="server">
    <Fisharoo:Folders id="Folders1"
        runat="server"></Fisharoo:Folders>
</asp:Content>
```

### Read message

Reading a message is not that difficult at all. We are just loading a message into a static UI based on the MessageID that is passed into the page. Let's first discuss the UI (which could easily be made more complex down the road!). This UI will consist of a From Label, a Subject Label, a Message Label, and a Reply button.

Here is the message view:

![](img/15a62e134135dde036b0ac3cbb01dab6_231_0.png)

And here is what will be seen after replying to the message:

![](img/15a62e134135dde036b0ac3cbb01dab6_232_0.png)

As usual, as all the leg work is done in the presenter, let's jump straight to it.

```csharp
//Fisharoo/Web/Mail/Presenters/ReadMessagePresenter.cs
public void Init(ReadMessage view)
{
    _view = view;
    _view.LoadMessage(_messageService.GetMessageByMessageID(_webContext.MessageID, _userSession.CurrentUser.AccountID));
}
```

As you can see here, we are populating the UI by getting the message based on the ID and then passing it to the view for display.

For the **Reply** button, we will bubble up the click event into the presenter. The presenter then makes a call to the Redirector class that passes the current MessageID to the NewMessage page.

## Problem

Apart from the standard infrastructure issues that we have to consider when building a system such as this, one of the core issues in any web-based file management system is file upload. As we all know, most server side technologies allow only one file to be uploaded at a time and ASP.NET is no different. And while we could easily buy a third-party plug-in to handle multiple files at once, we decided to provide for options to upload the files either via Silverlight or via Flash.

Once we get our file upload process working we are only one-third of the way there! As we are going to be mostly concerned with uploading images, we need to consider that we will need to provide some image manipulation. With each file that is uploaded to our system we need to create a handful of different sizes of each image to be used in various scenarios across our site. To start with, we will create thumbnail, small, medium, large, and original size photos.

Now while creating different size files is technically working with the file storage system, we wanted to take an extra breath with regards to the file storage concepts. As we discussed in the earlier chapters, we could choose to store the files on the file system or in a database. For avatars it made sense to store each with the profile data whereas for image galleries it makes more sense to store the file on the file system. While storing files to the file system we need to be very cautious as to how the file structure is defined and where and how the individual files are stored. In our case we will use system-generated GUIDs as our file names with extensions to define the different sizes that we are storing. We will dig into this more as we start to understand the details of this system.

Once we have uploaded the files to the server and they are ready for our use across the system, we will take up the concept of user files versus system files. If we build the system with some forethought regarding this topic we can have a very generic file management system that can be extended for future use. We will build a personal system in this chapter. But as you will see with some flags in just the right places, we could just as easily build a system file manager or a group file manager.

## Design

Let's take a look at the design for this feature.
