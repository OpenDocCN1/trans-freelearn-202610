
```aspx
Inherits="Fisharoo.Web.UserControls.ProfileDisplay" %>
<div style="height: 116px; float: left;">
    <table>
        <tr>
            <td style="width: 102px">
                <%-- NavigationURL and Image's URL will be set in code --%>
                <asp:HyperLink ID="imageLink" runat="server" Width="100" Height="100">
                    <asp:Image Style="padding: 5px; width: 100px; height: 100px;" ImageAlign="Left" Width="100" Height="100" ID="imgAvatar" ImageUrl="~/Images/ProfileAvatar/ProfileImage.aspx" runat="server" />
                </asp:HyperLink>
            </td>
            <td style="padding: 2px 2px 2px 15px; text-align: left;" valign="top">
                <asp:ImageButton ImageAlign="AbsMiddle" ID="ibInviteFriend" runat="server" AlternateText="Become Friends" OnClick="ibInviteFriend_Click" ImageUrl="~/Images/icon_friends.gif" ToolTip="Add As Friend" />
                <asp:ImageButton ImageAlign="AbsMiddle" ID="ibDelete" runat="server" OnClick="ibDelete_Click" ImageUrl="~/Images/icon_close.gif" ToolTip="Delete Friend" /><br />
                <asp:HyperLink ID="linkUsername" runat="server" /><br />
                <asp:Label ID="lblFirstName" runat="server"></asp:Label>
                <asp:Label ID="lblLastName" runat="server"></asp:Label><br />
                Since:
                <asp:Label ID="lblCreateDate" runat="server"></asp:Label><br />
                <asp:Label ID="lblFriendID" runat="server" Visible="false"></asp:Label>
            </td>
        </tr>
    </table>
</div>
```

```csharp
public void SendFriendRequest(Int32 AccountIdToInvite)
{
    _redirector.GoToFriendsInviteFriends(AccountIdToInvite);
}

public void DeleteFriend(Int32 FriendID)
{
    if (_userSession.CurrentUser != null)
    {
        _friendService.DeleteFriendByID(
            _userSession.CurrentUser.AccountID,
            FriendID);
        HttpContext.Current.Response.Redirect(
            HttpContext.Current.Request.RawUrl);
    }
}
```

The ProfileDisplay control also has a couple of properties—one to show/hide the delete friend button and the other to show/hide the invite friend button. These buttons are not appropriate for every page that the control is used in. In the search results page we want to hide the Delete button as the results are not necessarily friends. We would want to be able to invite them in that view. However, in a list of our friends the Invite button (to invite a friend) would no longer be appropriate as each of these users would already be a friend. The Delete button in this case would now be more appropriate.

Clicking on the Invite button makes a call to the Redirector class and routes the user to the InviteFriends page. An important point to note is the construction of the URL in the LoadDisplay() method, which takes you to the user's profile page.

You will recall that we had defined the ProfileRoute in previous chapter to support fancy URL directly linking to user's profile page. In order to create the link, we use the GetRouteURL helper method. This is useful if we end up changing our route pattern, as defined in Global.asax.cs, we don't need to make the changes to regenerate the URL. The GetRouteURL method will use the current route definition and return a valid URL.

## Invite your friends

This page allows us to manually enter email addresses of friends that we want to invite. It is a standard From, To, Message format where the system defaults the sender (to you), and you specify who to send to and the message that you want to send.

```csharp
}
public void SendInvitation(string ToEmailArray, string Message)
{
    string resultMessage = "Invitations sent to the following recipients:<BR>";
    Account account = _userSession.CurrentUser;
    resultMessage += _email.SendInvitations(account.AccountID, account.FirstName, account.LastName, ToEmailArray, Message);
    _view.ShowMessage(resultMessage);
    _view.ResetUI();
}
}
```

The UI is initialized based on the current user details. Once the user fills in the emails to be invited, the SendInvitation() method is invoked, that takes in the comma delimited array of emails and the message to be sent in the invitation. It then makes a call to the Email.SendInvitations() method.

```csharp
//Fisharoo/Components/ Email.cs
public string SendInvitations(int accID, string FirstName, string LastName, string ToEmailArray, string Message)
{
    string resultMessage = Message;
    foreach (string s in ToEmailArray.Split(','))
    {
        FriendInvitation friendInvitation = new FriendInvitation();
        friendInvitation.AccountID = accID;
        friendInvitation.Email = s;
        friendInvitation.GUID = Guid.NewGuid();
        friendInvitation.BecameAccountID = 0;
        _friendInvitationRepository.SaveFriendInvitation(
            friendInvitation);
        //add alert to existing users alerts
        Account account = _accountRepository.GetAccountByEmail(s);
        if (account != null)
        {
            _alertService.AddFriendRequestAlert(
                _userSession.CurrentUser, account,
                friendInvitation.GUID, Message);
        }
        SendFriendInvitation(s, FirstName, LastName,
                             friendInvitation.GUID.ToString(),
                             Message);
        resultMessage += "• " + s + "<BR>";
    }
    return resultMessage;
}
```

This method is responsible for parsing out all the emails, creating a new FriendInvitation, and sending the request via email to the person who was invited. It then adds an alert to the invited user if they already have an Account.

## Outlook CSV importer

### Importing contacts

The **Import Contacts** page is responsible for allowing our users to upload an exported contacts file from Microsoft Outlook into our system. Once they have imported their contacts the user is allowed to select which email addresses are actually invited into our system.

This page is made up of a couple of views, which are created using panels. This allows us to easily show or hide an entire set of controls within a panel. In the initial view we let the user upload a CSV file, which is an exported list of Microsoft Outlook contacts. There are instructions on the page that help the user with the export operations (see the following screenshot).

![](img/15a62e134135dde036b0ac3cbb01dab6_195_0.png)

### Selecting contacts

Once we have handled the upload process and parsed out the emails, we then need to display all the emails to the user so that they can select the ones they want to invite. This is another view/panel in the ASPX page.

We essentially have a checkbox list in our panel. This checkbox list is bound to the list of email addresses returned post-parsing in the presenter class.

```csharp
//Fisharoo/Web/Friends/OutlookCsvImporter.aspx.cs
public void ShowParsedEmail(List<string> Emails)
{
    pn1Upload.Visible = false;
    pn1Result.Visible = false;
    pn1Emails.Visible = true;
    cb1Emails.DataSource = Emails;
    cb1Emails.DataBind();
}
```

The user can go through the list and select the ones that they want to invite into our system. Once they are through selecting the emails that they want to invite, they can click on the Invite button.

We then iterate through all the items in the checkbox list to locate the selected items.

```csharp
//Fisharoo/Web/Friends/OutlookCsvImporter.aspx.cs
protected void btnInviteContacts_Click(object sender, EventArgs e)
{
    string emails = "";
    foreach (ListItem li in cb1Emails.Items)
    {
        if(li != null && li.Selected)
            emails += li.Text + ",";
    }
    emails = emails.Substring(0, emails.Length - 1);
    _presenter.InviteContacts(emails);
}
```

Once we have gathered all the selected emails we pass them to the presenter to run the invitation process.

```csharp
//Fisharoo/Web/Friends/PresentersOutlookCsvImporterPresenter.cs
public void InviteContacts(string ToEmailArray)
{
    string result = _email.SendInvitations(_userSession.CurrentUser,
                                           ToEmailArray, "");
    _view.ShowInvitationResult(result);
}
```
