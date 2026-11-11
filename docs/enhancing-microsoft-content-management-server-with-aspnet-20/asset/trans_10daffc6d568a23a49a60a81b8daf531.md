
修剪会自动由 PAPI 提供，因为只会检索当前用户有权查看的项目。

## 构建 MCMS 站点地图提供程序

要构建自定义的 SiteMapProvider，我们将创建一个继承自基类 SiteMapProvider 的类并实现基类继承的四个方法：

- The FindSiteMapNode() method returns a site map node based on a given URL or GUID.
- The GetChildNodes() method returns a collection of nodes of a specified parent node.
- The GetParentNode() method returns the parent node of a specified child.
- The GetRootNodeCore() method returns the first node in the hierarchy.

由于 TropicalGreen 站点的导航控件提供对频道项的链接，每个站点地图节点都映射到相应的频道项。尽管如此，您可以轻松修改方法以显示模板库项或资源库项。 现在，让我们继续构建 MCMSSiteMapProvider 类。

## MCMSSiteMapProvider 类

为了保持简单，我们可以利用 ASP.NET 中的另一个新功能：App_Code 文件夹。 App_Code 文件夹还允许我们在无需重建网站的情况下添加功能。

- 1. 首先，如果您尚未执行此操作，请将 App_Code ASP.NET 文件夹添加到 TropicalGreen 解决方案。右键单击项目并选择 添加 | ASP.NET 文件夹 | App_Code。
- 2. 在 App_Code 文件夹内创建一个名为 SiteMapProviders 的新子文件夹。
- 3. 右键单击新创建的 SiteMapProviders 文件夹并添加一个名为 MCMSSiteMapProvider.cs 的类文件。
- 4. 代码需要使用以下代码片段中突出显示的命名空间中的方法；因此将其添加到类文件中。 同时添加命名空间声明。

```
using System;
using System.Data;
using System.Configuration;
using System.Web;
using System.Web.Security;
```

```
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Web.UI.WebControls.WebParts;
using System.Web.UI.HtmlControls;
using Microsoft.ContentManagement.Publishing;

namespace TropicalGreen
{
    public class MCMSSiteMapProvider
    {
        . . . code continues . . .
    }
}
```

5. 由于没有可直接使用的与 MCMS 对象配合的 SiteMapProviders，自定义提供程序将继承自基类 SiteMapProvider 类。

```
public class MCMSSiteMapProvider : SiteMapProvider
{
    . . . code continues . . .
}
```

6. 我们还将添加一个名为 rootPath 的私有变量来存储顶级频道的路径。 对于 TropicalGreen，路径为 /Channels/tropicalgreen/。 在类声明中添加突出显示的代码。

```
public class MCMSSiteMapProvider : SiteMapProvider
{
    protected string RootChannel = "/Channels/tropicalgreen/";

    . . . code continues . . .
}
```

另外，我们可以将频道路径存储在 web.config 文件的 <appSettings> 部分中。这样可以使代码更易于在其他解决方案中重用，因为我们不必在路径更改时每次都重新编译代码。

7. 最后，删除类构造函数（代码如下）。它不是必需的。

```
public MCMSSiteMapProvider()
{
    //
    // TODO: Add constructor logic here
    //
}
```

## 实现 GetRootNodeCore() 方法

GetRootNodeCore() 方法返回层次结构中的第一个节点。在 TropicalGreen 站点的情况下，第一个节点是第一级频道 TropicalGreen 的节点。我们已经将它的路径存储在类变量 rootChannel 中。我们使用 PAPI 中的 Searches.GetByPath() 方法返回 TropicalGreen 频道的实例。将 GetRootNodeCore() 方法添加到类中。

```
protected override SiteMapNode GetRootNodeCore()
{
    Channel root = CmsHttpContext.Current.Searches.GetByPath
        (RootChannel) as Channel;
    return GetSiteMapNodeFromChannelItem(root);
}
```

请注意，在代码中，我们使用名为 GetSiteMapNodeFromChannelItem() 的方法基于频道项创建站点地图节点。站点地图节点有四个基本属性：

- **键**：用于唯一标识节点的数值。我们将将其设置为频道项的 GUID。
- **网址**：指定节点的地址，对于我们的站点将是频道项的 URL。
- **标题**：指定用于显示节点的文本标签。我们检索频道项的显示名称并将其用于标题。
- **描述**：指定当鼠标放置在节点上时显示的工具提示内容。特别适用于为访问者提供有关链接后面页面的描述。

从频道项，我们将基于前面列表中讨论的属性创建一个站点地图节点。将 GetSiteMapNodeFromChannelItem() 方法附加到类文件。

```
protected SiteMapNode GetSiteMapNodeFromChannelItem(ChannelItem ci)
{
    SiteMapNode smn = null;
    if (ci != null)
    {
        smn = new SiteMapNode(this, ci.Guid);
        smn.Url = ci.Url;
        smn.Title = ci.DisplayName;
        smn.Description = ci.Description;
    }
```

```
return smn;
}
```

## 实现 FindSiteMapNode() 方法

FindSiteMapNode() 方法根据给定的 URL 或 GUID 返回站点地图节点。我们将创建该方法的两个重载版本。

第一个接受一个字符串，该字符串可以是 GUID 或 URL，并返回一个节点。为了检测我们正在处理 URL 还是 GUID，我们查看字符串的第一个字符。如果它以左大括号 ({) 开头，那么它一定是 GUID。

从 GUID，我们使用 Searches.GetByGuid() 方法获取相应的频道项。对于 URL，我们调用辅助函数 EnhancedGetByUrl()。它与 Searches.GetByUrl() 执行相同的任务，但包括我们稍后将看到的若干增强功能。

一旦我们获得了频道项，我们将使用 GetSiteMapNodeFromChannelItem() 方法构造一个站点地图节点对象。

```
public override SiteMapNode FindSiteMapNode(string urlOrGuid)
{
    ChannelItem ci = null;
    if (urlOrGuid.StartsWith("{"))
    {
        ci = CmsHttpContext.Current.Searches.GetByGuid(urlOrGuid)
            as ChannelItem;
    }
    else
    {
        ci = EnhancedGetByUrl(CmsHttpContext.Current, urlOrGuid);
    }

    return GetSiteMapNodeFromChannelItem(ci);
}
```

第二个方法接受当前的 HttpContext 并返回代表当前正在访问的频道项的节点。

```
public override SiteMapNode FindSiteMapNode(HttpContext context)
{
    ChannelItem ci = CmsHttpContext.Current.ChannelItem;
    return GetSiteMapNodeFromChannelItem(ci);
}
```

## 增强 Searches.GetByUrl() 方法

将频道名称映射到主机标头是 MCMS 企业版中一个有用的功能。它使多个网站能够托管在同一服务器上。启用后，根频道下方的顶级频道也将成为主机标头。例如，如果根频道下方的频道名为 `tropicalgreen`，则该频道的 URL 将变为 `http://tropicalgreen`，而不是 `http://localhost/tropicalgreen`。

问题是 `Searches.GetByUrl()` 方法在启用映射主机标头时不可靠。当 `Searches.GetByUrl()` 方法接收到例如顶级频道的 URL `http://tropicalgreen` 时，我们期望它返回 `tropicalgreen` 频道的实例。然而，它返回 null 对象。这是因为 `Searches.GetByUrl()` 方法存在一个问题，导致它期望输入 URL 为 `http://localhost/tropicalgreen`，无论是否启用了将频道名称映射到主机标头名称。

作为变通方法，我们将增强 `Searches.GetByUrl()` 方法。我们首先检查是否已启用映射主机标头功能。为此，我们查看根频道的 URL。如果根频道的 URL 为 `http://Channels/`（URL 包含根频道本身的名称），我们就知道频道已被映射到主机标头。

如果已打开将频道名称映射到主机标头功能，我们将把 URL 转换为路径并使用 `Searches.GetByPath()` 方法获取频道项的实例。例如，如果 URL 为 `http://tropicalgreen/plantcatalog`，例程将其转换为频道的路径 `/Channels/tropicalgreen/plantcatalog`，并基于路径获取频道项的实例。将以下代码添加到类中：

```
private bool MapChannelToHostHeaderEnabled(CmsContext ctx)
{
    return (ctx.RootChannel.UrlModePublished == "http://Channels/");
}

private ChannelItem EnhancedGetByUrl(CmsContext ctx, string Url)
{
    if (MapChannelToHostHeaderEnabled(ctx))
    {
        string Path = HttpUtility.UrlDecode(Url);
        Path = Path.Replace("http://", "/Channels/");
        if (!Path.StartsWith("/Channels/"))
        {
            Path = "/Channels/" + HttpContext.Current.Request.Url.Host
```
