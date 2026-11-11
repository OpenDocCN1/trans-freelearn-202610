
一旦控件已添加到工具箱中，就将其拖放到 Plant.aspx 模板文件的任何可编辑区域。像之前为 TreeView 控件所做的那样设置 MCMSTreeview 的属性。页面渲染后，节点会按预期展开和折叠，即使在编辑模式下也不会出现离开警告消息。

完成后，删除植物模板文件中的所有 TreeView 控件。

## 使 TreeView 控件的按需填充功能在通道渲染脚本中工作

正如我们所见，按需填充功能在嵌入模板文件时可以正常工作。然而，如果你尝试将其添加到通道渲染脚本中，你可能会发现当展开任何节点时 TreeView 控件似乎会冻结。作为示例，让我们修改之前创建的 SiteMap.aspx 页面的 TreeView 控件：

- 1. 在源视图中打开 SiteMap.aspx 文件。
- 2. 按如下所示修改 TreeView 控件的属性：
- 3. 保存页面。

```
<asp:TreeView ID="TreeView1" runat="server" DataSourceID=
"SiteMapDataSourceTree"
ShowLines="True" PopulateNodesFromClient="True" ExpandDepth="1">
<DataBindings>
<asp:TreeNodeBinding PopulateOnDemand="true"
TextField="Title"
NavigateUrlField="Url"
ToolTipField="Description"/>
</DataBindings>
</asp:TreeView>
```

完成后，导航到站点地图页面。尝试展开任意节点。注意没有任何反应！

要理解为什么会发生这种情况，让我们仔细看看按需填充功能的工作方式。当节点展开时，浏览器会静默执行一次回发。回发需要 <form> 标签的 action 属性中存储的信息。如果你查看页面的生成的 HTML，你会发现表单的 action 属性中存储的值类似于以下内容：

```
<form name="aspnetForm" method="post" action="sitemap.aspx?...code continues...">
```

action 属性中存储的值有趣之处在于它是一个相对 URL。在常规 Web 应用程序中，使用相对 URL 工作良好。然而，对于 MCMS 页面，URL 必须是绝对的。这是因为用于访问页面的 URL（例如 http://tropicalgreen/sitemap/）并不指向物理文件夹——它纯粹是虚拟的，并指向存储库中的一个通道。因此当它尝试访问诸如 sitemap.aspx 的相对 URL 时，它会在 http://tropicalgreen/sitemap/sitemap.aspx 处查找它。由于文件在该位置物理上不存在，代码就会失败。

要纠正此问题，我们需要改用绝对链接。要在表单的 action 属性中生成绝对链接，我们可以选择：

- 1. 生成一个 <base> 标签并在其中存储页面的绝对路径。有了 <base> 标签，所有相对链接都会通过在链接前加上标签中存储的值而转换为绝对链接。通常，<base> 标签由 RobotMetaTag 控件生成。然而，控件中的一个错误阻止它在通道渲染脚本中生成 <base> 标签。一个增强版的 RobotMetaTag 可在通道渲染脚本和模板文件上同时工作，可在此处找到：http://www.gotdotnet.com/Community/UserSamples/Details.aspx?SampleGuid=8600bae5-7aba-481b-93bb-d179927179d5。
- 2. 使用客户端 JavaScript 修改 <form> 标签的 action 属性。虽然生成 <base> 标签是一种可能的解决方案，但你可能不想启用它（我们在第 5 章讨论了一些 <base> 标签不能使用的可能原因）。要使此方法工作，我们需要修改树视图的渲染方式，为此我们必须创建一个自定义 TreeView 控件。由于我们之前已经创建了一个自定义 TreeView 控件，让我们探讨如何增强它以在通道渲染脚本中工作。

![](img/18a0aa8d36427a4c069b30410f998b8e_150_0.png)

> TreeView 控件的按需填充功能之所以适用于植物模板文件，是因为我们已将 RobotMetaTag 控件的 RenderBaseHref 属性设置为 true。如果将其设置为 false，<base> 标签将不会生成，并且树视图将无法按需填充子节点。

我们需要将表单 action 属性中存储的相对链接转换为绝对链接。为此，我们将利用生成的页面中已有的一些功能。

如果你检查任何 MCMS 页面的 HTML 源代码，你会发现一个名为 __CMS_PostbackForm 的 JavaScript 变量，它存储了表单对象的实例。

```
var __CMS_PostbackForm = document.forms['aspnetForm'];
```

此外，还有一个名为 __CMS_CurrentUrl 的变量，它存储页面的绝对 URL。以下是站点地图页面的 __CMS_CurrentUrl 在你的页面上可能的样子。请注意，你站点上的 GUID 值可能不同。

```
var __CMS_CurrentUrl = "/tropicalgreen/templates/sitemap.aspx?
NRMODE=Published&
NRNODEGUID=%7b6bE3510D2-519A-4D4F-92A8-621B43F5894F%7d&
NRORIGINALURL=%2ftropicalgreen%2fSiteMap%2f&
NR_CACHEHINT=ModifyLoggedIn";
```

把这些片段组合起来，为了将 __CMS_CurrentUrl 中存储的值赋给表单、__CMS_PostbackForm 的 action 属性的值，我们需要在客户端以某种方式执行以下脚本：

```
<script type="text/javascript">
    if (typeof (__CMS_CurrentUrl) != "undefined")
    {
        __CMS_PostbackForm.action = __CMS_CurrentUrl;
    }
</script>
```

注册脚本的好地方是 TreeView 控件的 Render() 方法。打开 MCMS TreeView.cs 文件并将突出显示的代码添加到 Render() 方法中。

```
protected override void Render(System.Web.UI.HtmlTextWriter output)
{
    ... code continues ...
    // Register a client script that ensures that the action property
    // is correct when AJAX tries to do its postback.
    string script = "\n<script type=\"text/javascript\">\n" +
        " if (typeof (__CMS_CurrentUrl) != " +
        " \"undefined\")\n" +
        " {\n" +
        "   __CMS_PostbackForm.action = " +
        " __CMS_CurrentUrl;\n" +
        " }\n" +
        "</script>\n";
```

```
Page.ClientScript.RegisterStartupScript(this.GetType(),
    "AdjustActionForTreeView", script);
}
```

现在，用自定义的 MCMS TreeView 控件替换 SiteMap.aspx 上的 TreeView 控件。设置好按需填充节点后，树不再冻结，你应该能够展开和收缩节点以显示子节点。

## 在摘要页面中使用 TreeView 控件

摘要页面（也称为默认、封面或索引页面）提供给定部分内页面的列表。它们在访问者只知道页面的存在但不知道其确切位置时特别有用。例如，你可能知道植物目录中有一页描述芦荟，但你可能不知道其地址。如果植物目录页面提供了站点上所有植物页面的列表，你就可以轻松找到所需页面。

早期版本的摘要页面通常是手工编码的工作，当添加、删除或重命名新页面时必须频繁更新。多年来，随着活动脚本使开发人员能够即时动态组装摘要页面，这种做法逐渐被淘汰。这意味着页面显示的是列表的最新版本，为高效有效的链接管理提供了便利。

在基于 ASP.NET 1.x 构建的 MCMS 站点中，摘要页面通常通过遍历通道和帖子的集合来构建。然后将通道项的集合显示在类似表格的控件中，例如 DataGrid。虽然这种技术今天仍然有效，但 ASP.NET 2.0 引入了一种更快、更易于构建摘要页面的方法。

让我们看看一个替代方案，它涉及构建自定义站点地图提供程序和 TreeView 控件。以下是计划：

- 1. 我们首先创建一个自定义站点地图提供程序，将当前查看的通道设为根节点。然后将通道内的所有帖子添加为站点地图节点。
- 2. 接下来，我们将使用 TreeView 控件提供通道内所有页面的列表。虽然 TreeView 控件旨在显示项目的分层列表，但其多功能性也允许它同样好地显示简单的单级列表。
