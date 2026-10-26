## 使用 ROBOTS META 标签影响搜索引擎

通常，索引的大部分内容来自频道渲染脚本和发布项生成的页面。毕竟，它们是站点中最终用户可见的部分。要告诉搜索引擎哪些页面需要索引、哪些页面需要忽略，以及是否从当前页面继续跟踪链接，您可以在页面中添加一个名为 ROBOTS 的特殊 META 标签。大多数（但不是所有）搜索引擎会遵循此 META 标签，并评估其 content 属性，以确定是否应将页面添加到索引中，以及是否应跟踪此页面中的链接。

为了让 MCMS 作者能够利用此功能，频道和发布项有两个额外的属性：

- IsRobotIndexable
- IsRobotFollowable

这两个属性可用于生成 ROBOTS META 标签。如前所述，请注意某些搜索引擎选择忽略 ROBOTS META 标签。

下表总结了不同属性设置的行为：

| IsRobotFollowable | IsRobotIndexable | 要呈现的 META 标签 | 备注 |
|---|---|---|---|
| False | False | <meta name="ROBOTS" content="NOFOLLOW, NOINDEX"> | 识别机器人 META 标签的搜索引擎不会索引该页面，也不会跟踪其超链接。 |
| False | True | <meta name="ROBOTS" content="NOFOLLOW, INDEX"> | 识别机器人 META 标签的搜索引擎会索引该页面，但不会跟踪其超链接。 |
| True | False | <meta name="ROBOTS" content="FOLLOW, NOINDEX"> | 识别机器人 META 标签的搜索引擎不会索引该页面，但会跟踪其超链接。 |
| True | True | <meta name="ROBOTS" content="FOLLOW, INDEX"> | 识别机器人 META 标签的搜索引擎会索引该页面并跟踪其超链接。 |

有关 ROBOTS META 标签的更多信息，请查看此资源：

http://www.robotstxt.org/wc/exclusion.html#meta

## RobotMetaTag 控件和频道渲染脚本

MCMS 附带一个名为 RobotMetaTag 的控件，它会自动添加到所有由 Visual Studio .NET“MCMS 模板文件”模板创建的模板文件中。RobotMetaTag 控件用于根据页面的 IsRobotFollowable 和 IsRobotIndexable 属性生成 ROBOTS META 标签。

遗憾的是，此控件不适用于频道渲染脚本。如果将控件添加到频道渲染脚本中，即使设置了频道的 IsRobotFollowable 和 IsRobotIndexable 属性，也不会生成 ROBOTS META 标签。为了解决这个问题，让我们实现此控件的一个增强版本，使其同时适用于频道渲染脚本和模板。

> 在 MCMS 附带的 WoodgroveNet 示例站点中，RobotMetaTag 控件被包装在一个名为 HeadTags.ascx 的用户控件中。

首先，打开 Tropical Green 解决方案。将一个新类文件添加到 TropicalGreenControlLib 项目中，名称为 EnhancedRobotMetaTag.cs。

该控件需要使用以下突出显示命名空间中的方法，因此请将它们添加到类文件中。

```
using System;
using System.ComponentModel;
using System.Web.UI;
using Microsoft.ContentManagement.Common;
using Microsoft.ContentManagement.Publishing;
using Microsoft.ContentManagement.WebControls;
namespace TropicalGreenControlLib
{
    ... code continues ...
}
```





```csharp
protected override void Render( HtmlTextWriter writer )
{
    ChannelItem currentItem = CmsHttpContext.Current.ChannelItem;

    if( currentItem != null )
    {
        string followKey = (currentItem.IsRobotFollowable) ? "Follow" : "noFollow";
        string indexKey = (currentItem.IsRobotIndexable) ? "Index" : "noIndex";

        // write the robot META tag
        writer.Write("<meta name=\"ROBOTS\"\n"
                   + " content=\"" + followKey + "," + indexKey + "\">\n");

        if( this.RenderBaseHref )
        {
            // write the Base tag
            writer.Write("<base href=\"" + Page.Request.Url.AbsoluteUri + "\">\n");
        }
    }
}
```

保存并生成解决方案。像添加其他自定义控件一样，将该控件添加到工具栏中，然后就可以将其拖放到频道呈现脚本或模板文件中的`<head>`标签之间。

## 输出 META 标签

除了 ROBOTS META 标签之外，你还可以输出其他 META 标签，以帮助你所使用的特定搜索引擎进行索引。搜索引擎常用的标签包括 `<title>` 标签以及 Description 和 Keywords META 标签。

在 MCMS 中，频道和发布项具有 Description 和 DisplayName 属性，这些属性通常分别适用于 Description META 标签和 `<title>` 标签。

> 自定义属性也可以用作 META 标签，并呈现为 `<meta name="CustomPropertyName" content="CustomPropertyValue">`

作者可以输入关键词并将其存储为自定义属性值。接下来，你需要从存储库中检索这些值，并将它们作为 META 标签显示在页面的 head 区域中。

有几种方法可以实现这一点：

- 最快的方法是在模板 HTML 代码中直接使用 `<%= %>` 构造或 `Response.Write()` 调用。
- 如果内容位于自定义属性或占位符中，你可以使用代码隐藏文件来检索它。

与常规控件一样，使用 Web 用户控件可以在同一项目的模板文件之间共享代码。如果你需要在多个项目中存储的页面间生成同一组标签，可以考虑像前面为 ROBOTS 标签所做的那样编写一个自定义控件。

```csharp
using Microsoft.ContentManagement.Publishing;
... code continues ...

private void Page_Load(object sender, System.EventArgs e)
{
    // Put user code to initialize the page here
    Posting currentPosting = CmsHttpContext.Current.Posting;

    string keywords = "";
    CustomProperty cpKeywords = currentPosting.CustomProperties["Keywords"];
    if(cpKeywords!=null)
    {
        keywords = cpKeywords.Value;
    }

    litKeywords.Text = "<meta name=\"Keywords\" content=\"" + keywords + "\">";
}
```

### 重写 Web 用户控件的 Render 方法

这里有另一种方法：不用在 `<head>` 标签内使用 Literal 控件，你可以创建一个 Web 用户控件并重写其 Render() 方法。首先获取发布项的一个实例，然后获取其自定义属性值，最后创建 META 标签：

```csharp
namespace TropicalGreen.UserControls
{
    using System;
    using System.Data;
    using System.Drawing;
    using System.Web;
    using System.Web.UI.WebControls;
    using System.Web.UI.HtmlControls;

    // MCMS PAPI
    using Microsoft.ContentManagement.Publishing;

    // Namespace for HtmlTextWriter
    using System.Web.UI;

    public abstract class MetaTags : System.Web.UI.UserControl
    {
        private void Page_Load(object sender, System.EventArgs e)
        {
            // Put user code to initialize the page here
        }

        protected override void Render( HtmlTextWriter writer )
        {
            CmsHttpContext cmsContext = CmsHttpContext.Current;

            Posting p = cmsContext.Posting;
            string keywords = "";
            CustomProperty cpKeywords = p.CustomProperties["Keywords"];
            if (cpKeywords != null)
            {
                keywords = cpKeywords.Value;
            }
            writer.WriteLine("<meta name=\"keywords\" Content=\""
                           + keywords + "\">");
        }

        #region Web Form Designer generated code
        ... code continues ...
        #endregion
    }
}
```

## 为搜索索引准备发布项

我们刚刚创建的 MetaTags 用户控件还可以进行增强，将发布项的描述和自定义属性作为 META 标签输出。

> 你可以使用相同的技术将占位符内容添加到 META 标签中。MCMS Connector for SharePoint Technologies 包含可自定义的 SearchMetaTagGenerator 服务器控件，可从属性和自定义属性生成 META 标签。该控件将在第 5 章中讨论。

### 配置模板以允许发布项返回准确的最后修改时间

由于发布项是动态构建的，因此有关发布项最后修改日期的准确信息将始终反映当前日期和时间。因此，搜索引擎无法对 MCMS 站点执行增量抓取，而总是会进行完全抓取。

考虑一个使用 SharePoint Portal Server 搜索组件的网站。当 SharePoint Portal Server 执行增量索引抓取时，它会对所接触的站点上的每个页面发送 HTTP GET 请求。如果发现某个页面先前已被索引，则仅当该页面自上次索引作业以来被修改过时，SharePoint 才会更新其在索引中的条目。为了确定页面是否被修改，SharePoint 会发送一个条件 HTTP GET 请求，该请求在 `If-Modified-Since` HTTP 头中包含页面最后修改的日期和时间。如果页面未被修改，响应将是 HTTP 状态码 304（未修改），SharePoint 将不会对此页面进行索引。如果响应是 HTTP 状态码 200（正常），则表示页面已更新，SharePoint 将继续对其进行索引。这个概念对于由物理文件组成的静态网站很有效，因为其最后修改日期反映的是页面实际被修改的日期。

然而，由于其页面是即时组合的，IIS 将始终假定发布项的最后修改日期是当前日期和时间。由于存储在 `If-Modified-Since` HTTP 头中的日期总是更早的日期，IIS 总是返回状态码 200（正常）而不发送最后修改日期。因此，SharePoint Portal Server 永远不会对站点进行增量抓取，实际上每次都会进行完全索引。所有使用 `If-Modified-Since` HTTP 头执行增量抓取的搜索引擎都具有这种行为。

显然，这对于搜索引擎索引过程的性能来说并不理想，对 MCMS 站点的性能也不友好。以我们的 Tropical Green 站点为例，影响很小，因为发布项的总数非常低。然而，考虑一个包含 10,000 个发布项的站点，其中自上次抓取以来只有 10% 发生了变化。SharePoint Portal Server 索引的内容将是实际所需量的十倍，而不是抓取和索引 1,000 个页面。

要解决此问题，我们需要修改模板，以返回发布项实际修改的时间以及 HTTP 状态码 200；如果发布项自上次抓取以来未更改，则应返回 HTTP 状态码 304。我们将按以下步骤操作：

1. 获取发布项最后修改的日期。
2. 获取 HTTP `If-Modified-Since` 头返回的时间，并将其转换为 UTC 以消除时区的任何影响。

```csharp
return reAllSearch.Match(CurrentUserAgent).Success;
}
// agents are not specified in the web.config
return(false);
}
```

## 隐藏导航元素

对于 Tropical Green 解决方案中的每个导航控件，我们需要在其加载或呈现期间添加一项检查，以确定页面是否正被抓取。

最简单的方法是在控件的 Page_Load() 或 Render() 方法中执行此操作。我们要检查当前请求是否来自爬虫程序，如果是则隐藏该控件，否则正常加载：

```csharp
if (!TropicalGreen.Classes.SearchHelper.IsCrawler())
{
    // Bind the Data
}
else
{
    // Is being crawled so hide the user control
    this.Visible = false;
}
```

### 创建“发布项/频道列表”用户控件

既然我们已经隐藏了导航控件，仍然必须为爬虫程序提供一种遍历站点的机制。最简单的方法是构建一个用户控件，列出当前频道中的发布项和频道，但不包含其显示名称。

现在让我们构建 CrawlingNavigationControl 用户控件：

1. 在 Visual Studio .NET 中打开 TropicalGreen 项目。
2. 在用户控件文件夹下，创建一个新的用户控件，并将其命名为 CrawlingNavigationControl。
3. 切换到代码隐藏文件（CrawlingNavigationControl.ascx.cs）。
4. 导入以下命名空间：

   ```csharp
   using System.Text;
   using Microsoft.ContentManagement.Publishing;
   ```

我们将在所有模板和频道呈现脚本的 Page_Load() 中添加逻辑，以创建一个包含超链接的新 literal 控件。超链接标签将是索引器忽略的文本（噪音词）。在此情况下，我们使用单词"and"。

```csharp
private void Page_Load(object sender, System.EventArgs e)
{
    // Is the site being crawled?
    if(TropicalGreen.Classes.SearchHelper.IsCrawler())
    {
        // declare & instantiate a string builder to hold the hyperlinks
        StringBuilder sb = new StringBuilder();
        // loop through all the channel items in the current channel
        foreach(ChannelItem item in CmsHttpContext.Current.Channel.AllChildren)
        {
            // append the hyperlink creating a unique label
```




```
sb.Append("<a href=\"" + item.Url + "\">&and</a>");
}

// instantiate a new literal control
Literal litLinks = new Literal();

// set the literal controls text
// to be the text from the string builder
litLinks.Text = sb.ToString();

// add the literal control to the control collection
this.Controls.Add(litLinks);
}
}
```

要启用 CrawlingNavigationControl，你应将它添加到每个模板文件的底部。或者，如果你的站点在每个模板上都使用了页脚用户控件或页眉用户控件，你也可以将 CrawlingNavigationControl 放在其中。

## 搜索引擎优化技巧

大多数搜索引擎使用专有算法来对网页进行爬取、索引和排名。没有任何方法能保证你的网站获得高排名，但这里有一些通用的技巧可以帮到你。

## 设计与导航注意事项

有几个问题可能导致你的网站无法被搜索引擎正确索引：

## 框架

如果你的网站使用框架，框架内的内容可能不会被索引。要解决此问题，需要被索引的内容应放在 <NOFRAMES> 标签内。

## Flash 内容

如果你的首页主要以 Flash 为主，其中的内容和超链接可能无法被正确索引。建议在 HTML 中重现 Flash 中的内容和超链接，并且要么检测请求是否来自爬虫，要么把这些内容包含在所有页面中但将其隐藏。

## 图像映射与 JavaScript 导航

图像映射和由 JavaScript 填充的导航控件对开发者和用户来说都是很好的选择，但搜索引擎可能无法解析运行时生成的链接。如果你的网站使用图像映射或 JavaScript 生成超链接，建议用标准 HTML 重现这些链接。

## 为 Title 标签添加详细信息

<title> 标签是搜索引擎优化中最重要的标签之一。你应该让标题准确描述页面内容，因为它会显示为 Web 浏览器中的窗口标题，也会显示为搜索结果的标题。每个页面的标题应当唯一，并且每个关键词都应作为内容在页面中可见。此外，大多数搜索引擎对 title 标签中的词语评分高于出现在正文中的词语，因此当被搜索的词出现在 title 标签中时，该页面会在搜索结果列表中出现得更靠前。

## 使用 META 标签

META 标签允许搜索引擎在标准位置查找页面的详细信息。META 标签必须位于 <head> 元素内。

## 选择关键词与关键词密度

建议在你的页面中添加 <meta name="keywords" content="Your Keywords"> 标签，因为一些搜索引擎在构建索引时会使用此元素。content 元素包含一个逗号分隔的关键词列表。你应该使用与当前页面内容相关而非整个网站相关的关键词。Tropical Green 网站的关键词 META 标签示例为：

```
<meta name="keywords" content="Gardening information, Gardening Columns, plants for sale Seattle, Plant reseller Seattle, Plant supplier Seattle, Garden Designers Seattle, Online Plant Database, Garden design">
```

如果你选择过于通用的关键词，如 Plant、Gardening 或 Garden Design，它们的价值就会很低，因为不够具体。通过将 Tropical Green 的位置（西雅图）添加到每个关键词中，在本地（例如西雅图）搜索与植物相关服务的人更有可能找到 Tropical Green 的网站。

## 描述

description META 标签的内容应准确描述页面内容。例如，“A Walk in Jenny's Garden”栏目页面的描述可以是：

```
<meta name="description" content="We take a look at Jenny's Garden and her unique approach to garden design by combining the use of tall trees with small shrubs.">
```

keywords META 标签不应超过大约 1000 个字符，否则搜索引擎可能会忽略或截断它。为了获得最佳效果，应尽量保持 description META 标签简短（大约 250 个字符）。

## 本章小结

在本章中，我们研究了几种配置 MCMS 模板以充分利用搜索引擎的方法。为了让 MCMS 帖子和频道呈现脚本能够被搜索引擎索引和跟踪，应设置 IsRobotFollowable 和 IsRobotIndexable 属性，并且需要修改频道呈现脚本以输出这些属性。我们还探讨了如何为 MCMS 网站启用增量爬取，以及如何通过仅呈现相关内容并隐藏导航控件来减少搜索结果中的噪声。最后，我们介绍了一些搜索引擎优化技巧，包括为 META 标签添加详细信息，以及注意可能导致网站无法被正确索引的设计问题。
