

```
```csharp
{
    // authoring controls
    TextBox txbSharePointUrl;
    TextBox txbListName;
    LiteralControl litEventItemsLiteral;
    HtmlTable tblLayout;

    public SharePointEventsPlaceholderControl()
    {
    }
    - - - code continues - - -
```
```

接下来我们需要构建占位符控件的用户界面部分。我们将通过在我们重写的 `CreateAuthoringChildControls()` 方法中创建一个 HTML 表格来实现：

```
```csharp
protected override void CreateAuthoringChildControls(BaseModeContainer authoringContainer)
{
    // create our layout table
    tblLayout = new HtmlTable();

    // build row containing the SharePoint URL input prompt
    tblLayout.Rows.Add(BuildAuthoringSharePointSiteTableRow());

    // build row containing the SharePoint Event List name input prompt
    tblLayout.Rows.Add(BuildAuthoringSharePointEventListTableRow());

    // add table to the authoring container
    authoringContainer.Controls.Add(tblLayout);
}
```
```

现在我们需要创建这些 HTML 表格行的构建方法。我们首先需要的是创建 SharePoint 站点 URL 和列表名称输入的方法：

```
```csharp
private HtmlTableRow BuildAuthoringSharePointSiteTableRow()
{
    HtmlTableRow row;
    HtmlTableCell cell;

    // create sharepoint Web service label & input
    txbSharePointUrl = new TextBox();

    row = new HtmlTableRow();
    cell = new HtmlTableCell();
    cell.Controls.Add(new LiteralControl("SharePoint Portal Site URL:<br>"));
    cell.Controls.Add(txbSharePointUrl);
    row.Cells.Add(cell);

    // return the built row
    return row;
}

private HtmlTableRow BuildAuthoringSharePointEventListTableRow()
{
    HtmlTableRow row;
    HtmlTableCell cell;

    // create SharePoint event list name label & input
    txbListName = new TextBox();

    row = new HtmlTableRow();
    cell = new HtmlTableCell();
    cell.Controls.Add(new LiteralControl("SharePoint Event List Name:<br>"));
    cell.Controls.Add(txbListName);
```
```

```
row.Cells.Add(cell);
// return the built row
return row;
}
```

让我们看看我们的占位符控件目前的样子。构建 TropicalGreenControlLib 项目。假设没有错误，现在让我们将占位符控件添加到模板中。与其将其添加到现有模板，不如在 TropicalGreen 项目中创建一个新的模板文件（在 Templates 子目录下）并将其命名为 Events.aspx。使用 Homepage.aspx 模板作为 Events.aspx 页面的基础，但删除内容区域，以便最终的 Events 模板页面的 HTML 包含以下内容（注意我们添加了新的占位符控件的高亮部分）：

```
<%@ Page language="c#" Codebehind="Events.aspx.cs" AutoEventWireup="false" Inherits="TropicalGreen.Templates.Events" %>
<%@ Register TagPrefix="uc1" TagName="DefaultConsole" Src="../Console/DefaultConsole.ascx" %>
<%@ Register TagPrefix="cc1" Namespace="TropicalGreenControlLib" Assembly="TropicalGreenControlLib" %>
<%@ Register TagPrefix="uc1" TagName="RightMenu" Src="../UserControls/RightMenu.ascx" %>
<%@ Register TagPrefix="uc1" TagName="TopMenu" Src="../UserControls/TopMenu.ascx" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" >
<HTML>
<HEAD>
<title>Events</title>
<meta content="Microsoft Visual Studio .NET 7.1" name="GENERATOR">
<meta content="C#" name="CODE_LANGUAGE">
<meta content="JavaScript" name="vs_defaultClientScript">
<meta content="http://schemas.microsoft.com/intellisense/ie5" name="vs_targetSchema">
<LINK href="/tropicalgreen/Styles/Styles.css" type="text/css" rel="stylesheet">
</HEAD>
<body leftMargin="0" topMargin="0">
<form id="Form1" method="post" runat="server">
<table cellSpacing="0" cellPadding="0" width="100%" border="0">
<tr>
<td vAlign="top" width="100%" bgColor="#ffcc00" colSpan="2">
<IMG src="/tropicalgreen/images/Logo.gif">
</td>
<td vAlign="top" rowSpan="10">
<uc1:DefaultConsole id="DefaultConsole1" runat="server"></uc1:DefaultConsole>
</td>
</tr>
<tr bgColor="#66cc33">
<td colSpan="2">
<uc1:topmenu id="TopMenu1" runat="server"></uc1:topmenu>
</td>
</tr>
<tr>
<td style="PADDING-RIGHT: 30px; PADDING-LEFT: 30px; PADDING-BOTTOM: 30px; PADDING-TOP: 10px" vAlign="top">
<p>&nbsp;</p>
<b>Tropical Green Events</b>
<cc1:SharePointEventsPlaceholderControl id="SharePointEvents" runat="server" PlaceholderToBind="SharePointEventsConfig" />
</td>
<td class="RightMenuBar" vAlign="top"
```

```
align="center" width="20%" bgColor="#669900"
height="100%" rowSpan="2"
<uc1:rightmenu id="RightMenu1" runat="server"></uc1:rightmenu>
</td>
</tr>
</table>
</form>
</body>
</HTML>
```

一旦我们有了模板文件，我们需要在 MCMS 模板资源管理器中创建一个模板，以便使用我们的 Events 模板创建帖子。创建一个名为 Events 的新模板并将其放在 TropicalGreen 库下。

![](img/442e26bf28f2df81fbf8dff3478f88e3_204_0.png)

然后，创建一个单一的占位符定义，类型为 XmlPlaceholder，并将其命名为 SharePointEventsConfig。

![](img/442e26bf28f2df81fbf8dff3478f88e3_204_1.png)

最后，选择我们的新 Events.aspx 作为此新模板的模板文件。

![](img/442e26bf28f2df81fbf8dff3478f88e3_205_0.png)

签入模板并构建 TropicalGreen 项目。我们需要做的最后一件事是使用新的 Events 模板创建一个新的帖子。虽然它不会做太多，但它将帮助我们在构建占位符控件的每个部分时看到我们的控件变得活跃起来。通过浏览到 Tropical Green 站点的根目录创建新帖子，切换到编辑模式，使用 Events 模板创建一个新帖子，并以“Events”为名称和显示名称保存它。一旦帖子已保存，请继续批准它。

我们现在有一个可工作的占位符控件（尽管目前它没做什么），一个新的模板，以及我们在 Tropical Green 站点上的一个活动页面。在活动页面上（其 URL 为 http://www.tropicalgreen.net/TropicalGreen/Events.htm），点击 Web Author 控制台中的 Switch To Edit Site 链接，然后点击 Edit 以切换到该帖子的创作模式。你应该会看到我们为占位符创建的输入框和标签：

![](img/442e26bf28f2df81fbf8dff3478f88e3_205_1.png)

## 在 MCMS 和 SharePoint 之间发布内容

有了创作控件和一个用于查看我们进度的帖子，让我们继续为我们的占位符控件添加一些功能。无论帖子处于创作模式还是展示模式，我们都需要通过 SharePoint 列表服务 Web 服务检索 SharePoint 事件列表中的项目。为了访问此 Web 服务，我们需要服务的 URL、包含我们感兴趣的项目的列表的名称，以及该站点的有效登录凭据。

> 为简单起见，我们将对凭据进行硬编码。但是，我们强烈建议您采用更安全的解决方案，例如在站点的 Web.config 中加密用户名和密码，或允许内容所有者在占位符控件中与 URL 和列表名称一起输入凭据。Microsoft 建议使用 .NET 基类库中可用的数据保护 API（DPAPI）。

由于我们需要在两种模式下都使用此功能，我们将创建一个名为 `GetSharePointListItems()` 的方法，它需要 Web 服务 URL 和列表名称，同时返回列表中所有项目的数组。对于返回数组中的每个项目，我们希望了解有关该事件的以下信息：

- Title
- Description
- Location
- Start Date/Time
- End Date/Time

存储这些细节的最佳方式是在 `TropicalGreenControlLib` 项目中创建一个新类。将其命名为 `SharePointEventListItem.cs` 并添加以下代码以定义表示 SharePoint 事件列表中项目的业务对象：

```
using System;

namespace TropicalGreenControlLib
{
    public class SharePointEventListItem
    {
        private string _title;
        private string _location;
        private DateTime _startDateTime;
        private DateTime _endDateTime;

        // force callers to use the Factory method below
        private SharePointEventListItem() {}

        // Factory method used to create a new SharePointEventListItem
        public static SharePointEventListItem CreateItem(string title,
            string location, DateTime start, DateTime end)
        {
            SharePointEventListItem li = new SharePointEventListItem();
            li._title = title;
            li._location = location;
            li._startDateTime = start;
            li._endDateTime = end;
            return li;
        }
    }
```

