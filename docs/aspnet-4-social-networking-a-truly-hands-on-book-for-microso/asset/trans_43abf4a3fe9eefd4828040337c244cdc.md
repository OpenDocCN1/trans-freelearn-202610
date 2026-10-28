
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

ProfileDisplay 控件还有两个属性——一个用于显示/隐藏删除好友按钮，另一个用于显示/隐藏邀请好友按钮。这两个按钮并非适用于使用该控件的每个页面。在搜索结果页面中，我们希望隐藏“删除”按钮，因为搜索结果不一定是好友。在该视图中，我们希望能够邀请他们。然而，在好友列表中，“邀请”按钮（用于邀请好友）不再适用，因为这些用户都已经是好友了。在这种情况下，“删除”按钮反而更合适。

点击“邀请”按钮会调用 Redirector 类，并将用户路由到 InviteFriends 页面。需要注意的一个重要点是 LoadDisplay() 方法中 URL 的构造方式，它会带你进入用户的个人资料页面。

你会记得，我们在前一章中定义了 ProfileRoute，以支持直接链接到用户个人资料页面的友好 URL。为了创建该链接，我们使用 GetRouteURL 辅助方法。如果我们最终更改了路由模式（在 Global.asax.cs 中定义），这将非常有用，我们无需修改代码来重新生成 URL。GetRouteURL 方法将使用当前路由定义并返回有效 URL。

## 邀请好友

此页面允许我们手动输入想要邀请的好友的电子邮件地址。这是标准的“发件人、收件人、消息”格式，系统默认发件人为你（当前用户），你可以指定收件人以及想要发送的消息。

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

UI 会根据当前用户的详细信息进行初始化。当用户填写完要邀请的电子邮件后，会调用 SendInvitation() 方法，该方法接收逗号分隔的电子邮件数组以及要在邀请中发送的消息。然后它会调用 Email.SendInvitations() 方法。

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

此方法负责解析所有电子邮件、创建新的 FriendInvitation，并通过电子邮件向被邀请者发送请求。然后，如果被邀请者已有账户，则会向其添加一条提醒。

## Outlook CSV 导入器

### 导入联系人

**导入联系人**页面允许我们的用户将 Microsoft Outlook 导出的联系人文件上传到我们的系统。导入联系人后，用户即可选择实际要邀请到我们系统中的电子邮件地址。

此页面由两个视图组成，这些视图是使用面板创建的。这使我们能够轻松地显示或隐藏面板中的整套控件。在初始视图中，我们允许用户上传 CSV 文件，该文件是 Microsoft Outlook 联系人的导出列表。页面上有帮助用户执行导出操作的说明（参见下面的截图）。

![](img/15a62e134135dde036b0ac3cbb01dab6_195_0.png)

### 选择联系人

处理完上传过程并解析出电子邮件后，我们需要将所有电子邮件显示给用户，以便他们可以选择想要邀请的邮件。这是 ASPX 页面中的另一个视图/面板。

我们的面板中有一个复选框列表。该复选框列表绑定到 Presenter 类中解析后返回的电子邮件地址列表。

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

用户可以浏览该列表，选择想要邀请到我们系统中的邮箱。选择完想要邀请的邮箱后，他们可以点击“邀请”按钮。

然后，我们遍历复选框列表中的所有项，以定位已选中的项。

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

收集完所有选中的电子邮件后，我们会将其传递给 Presenter 以执行邀请流程。

```csharp
//Fisharoo/Web/Friends/PresentersOutlookCsvImporterPresenter.cs
public void InviteContacts(string ToEmailArray)
{
    string result = _email.SendInvitations(_userSession.CurrentUser,
                                           ToEmailArray, "");
    _view.ShowInvitationResult(result);
}
```

