
完成后，植物目录频道的摘要将如下所示：

- Aloe Vera
- Banana Tree
- Bougainvillea
- Coconut Tree
- EggPlants
- Ficus
- Hibiscus
- Ixora
- Man Eating Plant

## SiteMapProviderSummary 类

让我们开始创建自定义站点地图提供程序类，它将与之前创建的 `MCMS SiteMapProvider` 类相似，有几点不同：站点地图不再包含网站上所有频道的层级列表，而是从（并且仅包含）当前查看的频道开始。其次，只有当前频道内的页面才会被添加到站点地图中。

- 1. 在 **TropicalGreen** 项目的 `App_Code/SiteMapProviders` 文件夹中添加一个类文件。将文件命名为 **MCMS SiteMapProviderSummary.cs**。
- 2. 我们需要在以下代码中使用的命名空间中的方法。添加突出显示的命名空间以及命名空间声明。

```
using System;
using System.Data;
using System.Configuration;
using System.Web;
using System.Web.Security;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Web.UI.WebControls.WebParts;
using System.Web.UI.HtmlControls;
using Microsoft.ContentManagement.Publishing;

namespace TropicalGreen
{
    . . . code continues . . .
}
```

- 3. 与我们构建的其他自定义站点地图提供程序一样，我们不会从头编写它。我们将走捷径并继承之前创建的 MCMSSiteMapProvider 类。

```
public class MCMSSiteMapProviderSummary : MCMSSiteMapProvider
{
    . . . code continues . . .
}
```

- 4. 由于列表显示的是单个频道的内容，因此我们无需将其他所有频道添加到站点地图中。相反，我们将当前查看的频道设为根站点地图节点路径。为此，我们重写 GetRootNodeCore() 方法。

```
protected override SiteMapNode GetRootNodeCore()
{
    // Set the currently viewed channel to be the root node
    Channel root = CmsHttpContext.Current.Channel;
    return base.GetSiteMapNodeFromChannelItem(root);
}
```

- 5. 接下来，我们将重写 GetChildNodes() 方法。我们检索当前频道内的所有页面的集合，并按显示名称按字母顺序排序。然后我们将它们逐个添加到站点地图中。

```
public override SiteMapNodeCollection GetChildNodes(SiteMapNode node)
{
    // Create a new SiteMapNodeCollection
    SiteMapNodeCollection smnc = new SiteMapNodeCollection();

    // Get an instance of the currently viewed channel
    Channel channel =
        CmsHttpContext.Current.Searches.GetByGuid(node.Key) as Channel;

    if (channel != null)
    {
        // Get a collection of all postings within the channel
        PostingCollection pc = channel.Postings;

        // Sort the postings by Display Name (from A-Z)
        pc.SortByDisplayName();
```

```
// Add each posting to the site map
foreach (Posting p in pc)
{
    smnc.Add(GetSiteMapNodeFromChannelItem(p));
}
}
return smnc;
}
```

- 6. 类已完成。保存 MCMSSiteMapProviderSummary 类文件。
- 7. 与之前一样，在使用之前，我们需要在 web.config 文件中添加对自定义站点地图提供程序的引用。将突出显示的行添加到 web.config 文件的 <providers> 部分。

```
<providers>
    . . . code continues . . .
    <add name="MCMSSiteMapProviderSummary"
        type="TropicalGreen.MCMSSiteMapProviderSummary"/>
</providers>
```

## 配置 TreeView 控件以显示平面项目列表

现在我们已经创建了提供给定频道内所有页面列表的自定义站点地图提供程序，我们需要找到一个能够显示该列表的控件。开箱即用，我们可以选择使用 SiteMapPath、Menu 或 TreeView 控件。为本示例的目的，让我们选择 TreeView 控件。

使用 TreeView 控件来显示平面项目列表似乎违反直觉。毕竟，它被设计用来显示层级列表。尽管如此，我们将看到 TreeView 控件是一个非常多用途的控件，能够显示层级嵌套项目以及单维平面列表。

- 1. 首先，在 TropicalGreen 项目的 /Templates/ 文件夹中创建一个新的 Web 表单。将文件命名为 ChannelRenderingScript.aspx。选择要选择母版页。
- 2. 在“选择母版页”对话框中，选择位于 Templates 文件夹中的 tropicalgreen.master 文件。
- 3. 在设计视图中打开 ChannelRenderingScript.aspx，从工具箱的数据部分拖动 SiteMapDataSource 控件并将其放置在内容区域的任何位置。为控件赋予以下属性值：

| 属性 | 值 |
|---|---|
| ID | SiteMapDataSourceSummary |
| SiteMapProvider | MCMSSiteMapProviderSummary |
| ShowStartingNode | False |

我们已将 SiteMapDataSource 设置为链接到我们之前构建的站点地图提供程序：MCMSSiteMapProviderSummary。注意，由于我们不希望当前查看的频道作为列表的一部分显示，因此我们将 ShowStartingNode 属性设置为 false。

- 4. 接下来，从工具箱的导航部分拖动 TreeView 控件并将其放置在内容区域的任何位置。为其赋予以下属性值：

| 属性 | 值 |
|---|---|
| ID | TreeView1 |
| DataSourceID | SiteMapDataSourceSummary |
| MaxDataBindDepth | 1 |
| ShowExpandCollapse | False |

通过将 MaxDataBindDepth 属性设置为 1，我们确保 TreeView 控件永远不会显示超过一个层级的项目。这样列表始终是平面的。此外，我们将 ShowExpandCollapse 设置为 false 以防止展开的加号 [+] 和减号 [-] 符号出现。

- 5. 最后，为了在列表中的每个项目旁边显示一个项目符号，我们将手动定义每个节点的绑定。切换到源视图并在 <asp:TreeView> 标签之间添加突出显示的代码。

```
<asp:TreeView ID="TreeView1" runat="server" DataSourceID="SiteMapDataSourceSummary" MaxDataBindDepth="1" ShowExpandCollapse="False">
<DataBindings>
<asp:TreeNodeBinding TextField="Title" NavigateUrlField="Url" ToolTipField="Description" FormatString='<li>{0}</>' />
</DataBindings>
</asp:TreeView>
```

我们已将树视图节点绑定到站点地图节点的 Title、Url 和 Description 属性。此外，通过将 FormatString 设置为包含 <li> 标签，我们包含了要在每个项目旁边打印的项目符号。

- 6. 页面已完成。保存并编译网站。

接下来，让我们将 ChannelRenderingScript.aspx 文件分配为植物目录频道的频道渲染脚本。

- 1. 在站点管理器中，右键单击 PlantCatalog 频道。选择属性。
- 2. 在 PlantCatalog 属性对话框中，单击“发布”选项卡。
- 3. 在“频道渲染”部分，单击“选择”按钮。
- 4. 在“选择频道渲染”对话框中，在脚本 URL 字段中输入 /tropicalgreen/templates/ChannelRenderingScript.aspx。
- 5. 单击“确定”并关闭所有打开的对话框。

这样，我们就可以查看摘要页面了。导航到 http://tropicalgreen/plantcatalog 并查看频道内的所有植物页面列表。

频道渲染脚本 ChannelRenderingScript.aspx 可以应用于网站上的所有其他频道。请继续将其分配为 Columns 和 Gardens 频道的频道渲染脚本。

## 摘要

使用随 ASP.NET 2.0 提供的导航控件，构建站点导航从未如此简单。在本章中，我们首先构建了一个自定义 SiteMapProvider，它从 MCMS 仓库读取以填充站点地图。接下来，我们展示了 SiteMapPath 控件如何与自定义 SiteMapProvider 配合工作，以提供面包屑链接，显示用户在站点中的确切位置。我们看到 SiteMapPath 控件正常工作不需要额外的编程。随后，我们研究了 Menu 控件。我们构建了该控件的两个版本——一个放置在页面顶部的静态水平菜单，以及一个位于页面右侧的动态垂直菜单。最后，我们演示了 TreeView 控件如何与 SiteMapProvider 一起使用，以提供站点层级的全面视图。我们展示了如何配置它以按需填充节点，以及作为频道渲染脚本的一部分来显示特定频道内所有帖子的摘要列表。
