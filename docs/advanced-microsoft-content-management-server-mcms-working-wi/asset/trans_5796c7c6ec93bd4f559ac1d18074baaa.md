

## 在演示模式下呈现 RSS Feed

我们现在将创建用于呈现 RSS Feed 的界面。在未发布模式下，RssReader 占位符控件将显示“Feed will be shown in published mode”，并且当占位符在已发布模式下查看时，LoadAndRenderRssFeed() 将被执行以检索 feed 并将其输出绑定到 basePresentationControl。

首先我们重写 CreatePresentationChildControls() 来创建演示子控件：

```csharp
#region Presentation

/// <summary>
/// Create the Presentation Controls
/// </summary>
/// <param name="presentationContainer"></param>
protected override void CreatePresentationChildControls(
    BaseModeContainer presentationContainer)
{
    // Instantiate a literal control and
    // set the base presentation control
    this.basePresentationControl = new LiteralControl();

    // Set the ID of the base presentation control
    this.basePresentationControl.ID = "PresentationControl";
    // Add the base presentation control to
    // the presentation container
    presentationContainer.Controls.Add(this.basePresentationControl);
}
```

```csharp
// Date?
if (this.ShowDate)
{
    // Append the Date
    sb.Append(PubDate);
}

sb.Append("</div>\n");

// Are we showing the Open Article Link?
if (this.ShowOpenArticleLink)
{
    sb.Append(" <div>");
    sb.Append("<a target=\"_blank\" href=\"" + Link
        + "\">[Open Article]</a>");
    sb.Append("</div>\n");
}

sb.Append("</div>\n");
rsReader.Read();
break;
}
default:
{
    rsReader.Read();
    break;
}
}
}

// return the string builder contents
return(sb.ToString());
}

#endregion
```

要加载 RSS feed 并使用占位符控件输出项目，我们将重写 LoadPlaceholderContentForPresentation() 方法。feed 在“Presentation Unpublished”模式下不会被呈现，以提高作者效率。

为了最小化对 feed 提供者的影响，我们将为占位符控件实现内容缓存。这意味着即使在配置的时间范围内有多个用户请求包含 RssReaderPlaceholderControl 的帖子，也只有一个 Web 请求会命中 feed 站点。这最大限度地减少了 feed 站点以及托管此占位符控件的网站的流量和处理开销。

缓存持续时间目前硬编码为五分钟。如果您需要更灵活，可以将缓存持续时间存储在 Web.config 中，或在作者模式中添加一个文本框来捕获缓存持续时间并将其保存为 XmlPlaceholder 的附加配置值。

```csharp
/// <summary>
/// Load and parse the RSS feed. Output the feed
/// items to the basePresentationControl
/// </summary>
/// <param name="e"></param>
protected override void
LoadPlaceholderContentForPresentation(PlaceholderControlEventArgs e)
{
    // Verify the child controls have been created
    EnsureChildControls();

    try
    {
        // Check the current mode
        if (WebAuthorContext.Current.Mode ==
            WebAuthorContextMode.PresentationUnpublished)
        {
            this.basePresentationControl.Text =
                "Feed will be shown in published mode";
        }
        else
        {
            // Load the settings from the XML
            this.LoadSettingsFromXml(this.BoundXmlPlaceholder.XmlAsString);

            // Get the current Cache object from HttpContext
            System.Web.Caching.Cache cache = HttpContext.Current.Cache;

            // we will vary the cache entry by the content of the
            // current placeholder object. This will ensure that different
            // feeds can be served from the same website
            string CacheKey = this.BoundXmlPlaceholder.XmlAsString;

            // check if content is already in Cache
            string cacheXml = cache["RssReader." + CacheKey] as string;
            if (cacheXml == null)
            {
```

## 对 RSS Feed 生成器的增强

- 作者姓名—该解决方案可以通过与 Active Directory 集成来检索作者的完整姓名，从而得到增强。
- 基于频道的 feed 生成—将频道属性设置为配置参数以供 feed 使用，并从帖子或频道渲染脚本生成它。
- 存储历史记录—组织可能需要保留其网站上所有历史内容的副本。RSS feed 目前每 5 分钟可能发生变化一次（当缓存过期时）。为了使 RSS feed 充当出现在页面修订历史记录中的内容，可以在保存时发出对 feed 的请求，并将生成的 HTML 保存到 HtmlPlaceholder 中。然后可以编写一个自动更新工具，在计划的时间，例如每 60 分钟，更新聚合的 feed。

## 对 RSS 聚合占位符控件的增强

- 设计—RSS 聚合控件仅具有基础的外观和感觉，可以通过向 div 元素添加 class 属性来轻松增强，以应用页面 CSS 样式表中定义的样式。

## 总结

我们现在已为 Tropical Green 网站提供了通过 RSS 同步和聚合内容的能力。内容作者将能够指定要在特定帖子上聚合和显示的 RSS feed。最终用户现在有机会订阅我们 MCMS 网站上的 RSS feed，以便在内容发生变化时自动收到通知，而无需浏览网站。此外，MCMS 网站现在可以从互联网上的其他服务器聚合 RSS feed 的内容并显示其自身网站的部分内容。

在 Current Version 标题下方，我们将添加包含指向帖子工作版本的超链接的表格。切换到设计视图。从菜单栏中选择 Table | Insert | Table... 并插入一个包含一行两列的表格。在左侧列的表格单元格中，添加单词 Name。将 HyperLink 控件（来自工具箱的 Web Forms 部分）拖放到右侧单元格中。为 HyperLink 指定 ID aName（请注意，我们已在前面加上字母 'a'），并将其 Text 属性设置为空字符串。

接下来，将 DataGrid 拖动到 Available Revisions 标题下方。DataGrid 将列出帖子的所有修订。将 DataGrid 的 ID 设置为 dgPageRevisionsList，并将 AutoGenerateColumns 属性设置为 false。切换到 HTML 视图并向网格添加四列。第一列显示该特定版本的帖子名称，第二列显示修订日期，第三列是链接到预览页面的超链接，第四列是触发页面还原过程的按钮。

```xml
<asp:DataGrid ID="dgPageRevisionsList" Runat="server"
AutoGenerateColumns="False">
<Columns>
<asp:BoundColumn DataField="Name" HeaderText="Name"></asp:BoundColumn>
<asp:BoundColumn DataField="RevisionDate" HeaderText="Revision Date">
</asp:BoundColumn>
<asp:HyperLinkColumn Text="Preview" Target="CMSPageRevertPreviewWindow" DataNavigateUrlField="Url">
</asp:HyperLinkColumn>
<asp:ButtonColumn Text="Revert" CommandName="RevertTo">
</asp:ButtonColumn>
</Columns>
</asp:DataGrid>
```

切换到设计视图。将两个 CheckBox 控件拖动到 DataGrid 上方。将第一个 CheckBox 的 ID 设置为 chkRevertContent，其 Text 属性设置为 Revert Placeholder Content。第二个 CheckBox 的 ID 为 chkRevertCustomProperties，Text 值为 Revert Custom Property Values。将两个 CheckBox 的 Checked 属性都设置为 true。

拖动一个 Label 控件并将其放置在 DataGrid 下方。将 Label 的 ID 设置为 lblMessage，其 Text 属性设置为空字符串。我们将在此处显示来自应用程序的消息（例如错误）。

您应该看到以下内容：

