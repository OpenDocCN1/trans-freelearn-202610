

## 消息

Translator - AccountsToUserNames 接收账户集合并返回这些账户的用户名字符串，这正是最终绑定到 UI 中 repeater 的内容。

在我们 friend 控件的代码后置中，我们插入一段 JavaScript 片段，以便我们可以点击一个好友并将其用户名带入我们 NewMessage UI 中的 To 字段。

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

请注意，我们为链接的 OnClick 事件添加了一个属性，该属性将调用 JavaScript 以将用户名移动到我们的 To 字段，并以分号作为分隔符。

这为我们提供了一个好友列表，但我们如何将其关联回我们的 UI？打开 NewMessage.aspx 页面。我们需要在页面中注册新的控件，然后在页面中添加对其的引用。

在 Page 指令正下方添加以下代码：

```
<%@ Register Src="~/Mail/UserControls/Friends.ascx"
             TagPrefix="Fisharoo" TagName="Friends" %>
```

然后在我们的页面中添加一个新的 Content 部分，好友控件将位于此处：

```
<asp:Content ContentPlaceHolderID="LeftNavTop" runat="server">
    <Fisharoo:Friends ID="friends1" runat="server" />
</asp:Content>
```

这会将好友控件插入到主页面中定义的左侧导航栏中！

将消息加载到 UI 中只是一个将 DataSource 绑定到 repeater 的问题。

```csharp
public void LoadMessages(List<PMessage> Messages)
{
    repMessages.DataSource = Messages;
    repMessages.DataBind();
}
```

由于这不需要进一步解释，让我们直接进入构建页面导航的方法。回想一下，在我们 presenter 的 Init() 中，我们调用了 _view.DisplayPageNavigation()，它接收我们正在处理的文件夹的 PageCount 以及我们正在查看的当前页面。以下是该方法：

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

这段代码与我们 UI 中的三个控件交互——两个超链接，一个显示 **Previous**，一个显示 **Next**，以及一个 **PlaceHolder** 控件，该控件将保存此数据集所有页面的各个页码。

该方法首先根据当前页面和总页数确定是否应显示 **Next** 或 **Previous** 链接。然后我们将每个链接的导航属性挂钩，以便带我们进入下一页或上一页数据。之后我们使用 for 循环遍历所有可能的页面，从 1 到 PageCount，为每次迭代创建一个新的超链接，其中包含该页面的位置。

此页面的另一个功能是能够删除消息——我们可以删除任意多或任意少的消息。这主要通过视图代码后置中的一个辅助函数实现，该函数提取所有选中的消息，可以从 presenter 中调用。这允许 presenter 保持控制！

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

此方法遍历所有复选框以查看它们是否被选中。如果被选中，则从我们在加载显示时创建的属性中提取 MessageID。然后将 MessageID 集合返回给调用者。

这就把我们带到了 presenter 中的 **Delete** 方法。它从冒泡到 presenter 的按钮点击事件中调用，然后调用视图获取选中的 MessageID 列表。然后使用 MessageService 获取该消息的副本。有了副本后，我们调用 MessageRecipientService.DeleteMessageRecipient() 方法并删除每个选中的消息。

```csharp
linkFolder.NavigateUrl = "~/Mail/Default.aspx?folder=" +
    ((MessageFolder)e.Item.DataItem).MessageFolderID.ToString();
linkFolder.Attributes.Add("FolderID",
    ((MessageFolder)e.Item.DataItem).MessageFolderID.ToString());
}
}
```

这为我们在一个控件中创建了一个文件夹列表。但我们如何将其放入我们的 UI 中？为此，我们必须返回到我们的 Default.aspx 页面。这将与我们在新消息页面中为好友控件所做的完全相同。打开默认页面，以便我们可以添加 Folders 控件。

```
<%@ Register Src="~/Mail/UserControls/Folders.ascx"
    TagPrefix="Fisharoo" TagName="Folders" %>
<asp:Content ContentPlaceHolderID="LeftNavTop" runat="server">
    <Fisharoo:Folders id="Folders1"
        runat="server"></Fisharoo:Folders>
</asp:Content>
```

### 读取消息

读取消息并不困难。我们只是根据传递到页面的 MessageID 将消息加载到静态 UI 中。让我们先讨论 UI！该 UI 将由一个 From 标签、一个 Subject 标签、一个 Message 标签和一个 Reply 按钮组成。

以下是消息视图：

![](img/15a62e134135dde036b0ac3cbb01dab6_231_0.png)

以下是回复消息后会看到的内容：

![](img/15a62e134135dde036b0ac3cbb01dab6_232_0.png)

与往常一样，所有繁重的工作都在 presenter 中完成，让我们直接进入它。

```csharp
//Fisharoo/Web/Mail/Presenters/ReadMessagePresenter.cs
public void Init(ReadMessage view)
{
    _view = view;
    _view.LoadMessage(_messageService.GetMessageByMessageID(_webContext.MessageID, _userSession.CurrentUser.AccountID));
}
```

正如你在这里看到的，我们通过根据 ID 获取消息然后将其传递给视图以供显示来填充 UI。

对于 **Reply** 按钮，我们将点击事件冒泡到 presenter。Presenter 然后调用 Redirector 类，将当前 MessageID 传递到 NewMessage 页面。

## 问题

除了构建此类系统时我们需要考虑的标准基础设施问题之外，任何基于 Web 的文件管理系统中的核心问题之一都是文件上传。正如我们所知，大多数服务器端技术一次只允许上传一个文件，ASP.NET 也不例外。虽然我们可以轻松购买第三方插件来同时处理多个文件，但我们决定提供通过 Silverlight 或通过 Flash 上传文件的选项。

一旦我们的文件上传流程运行起来，我们才完成了三分之一的工作！由于我们将主要关注上传图像，因此我们需要考虑提供一些图像处理。每次文件上传到我们的系统时，我们都需要为每个图像创建多个不同尺寸，以便在我们网站的各个场景中使用。起初，我们将创建缩略图、小型、中型、大型和原始尺寸照片。

现在，虽然创建不同尺寸的文件在技术上是与文件存储系统一起工作，但我们希望对文件存储概念做进一步的思考。正如我们在前几章中所讨论的，我们可以选择将文件存储在文件系统或数据库中。对于头像，将其与个人资料数据一起存储是有意义的，而对于图像库，将文件存储在文件系统上更有意义。在将文件存储到文件系统时，我们需要非常谨慎地定义文件结构以及文件存储的位置和方式。在我们的例子中，我们将使用系统生成的 GUID 作为文件名，并使用扩展名来定义我们存储的不同尺寸。我们将在开始了解该系统的细节时更深入地探讨这一点。

一旦我们将文件上传到服务器并准备好在整个系统中使用，我们将讨论用户文件与系统文件概念。如果我们在构建系统时对这个主题有所考虑，我们就可以拥有一个非常通用的文件管理系统，可以在未来进行扩展。我们将在本章中构建一个个人系统。但正如你将在适当位置看到的一些标志，我们同样可以轻松构建一个系统文件管理器或一个群组文件管理器。

## 设计

让我们来看看此功能的設計。

