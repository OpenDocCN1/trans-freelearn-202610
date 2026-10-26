

RemoveTagPair() 方法会移除 `<a></a>` 标签，但保留标签之间的文本。例如，如果链接是：`<a href="/NR/rdonlyres/8C78AC41-591D-4626-AFC8-19AC8375914E/0/myresource.gif">My Resource </a>`，那么对这段文本运行 RemoveTagPair() 方法后，只会留下字符串 My Resource。

```csharp
private static string RemoveTagPair(string text, string tag, string endtag, string recursecompare)
{
    // Get the first occurrence of the <a> tag within the content
    int start = text.ToLower().IndexOf(tag.ToLower());

    // Initialize the end point to -1
    int end = -1;

    while (start >= 0)
    {
        // Find the end point
        end = FindTag(text, endtag, start, recursecompare);

        // Got the end point. Remove the end tag, </a>
        if (end > start)
        {
            text = text.Remove(end, endtag.Length);
        }

        // Remove the start tag <a>
        text = text.Remove(start, tag.Length);

        // Continue the search for <a></a> tags
        start = text.ToLower().IndexOf(tag.ToLower());
    }
    return text;
}
```

FindTag() 方法用于处理嵌套的锚点标签。我们可能会遇到一个锚点标签嵌套在另一个锚点标签中的情况，如下所示：

`<a href="/NR/rdonlyres/8C78AC41-591D-4626-AFC8-19AC8375914E/0/myresource.gif">My Resources @ <a href="http://www.mywebsite.com">MyWebSite</a>. Check it out now!</a>`

虽然嵌套锚点标签并不是良好的编程实践，但这种情况仍有可能发生，尤其是在作者使用内置的所见即所得编辑器，而没有机会查看 HTML 代码效果的时候。

FindTag() 通过检查是否存在多个 `<a></a>` 标签对来处理嵌套标签。如果存在，它会继续搜索，直到找到与第一个 `<a>` 标签匹配的最终结束标签 `</a>`。请将 FindTag() 方法直接添加到 RemoveTagPair() 方法下方。

```csharp
private static int FindTag(string text, string tag, int start, string recursecompare)
{
    // Get the position of the next </a> tag
    int pos = text.ToLower().IndexOf(tag.ToLower(), start);

    // Check to see if there are nested hyperlinks
    int check = text.ToLower().IndexOf(recursecompare.ToLower(), start+1);

    // Found a nested <a></a> tag
    if ((check >= 0) & (check < pos))
    {
        // Keep searching until we find the </a> tag that matches
        // the first <a> tag
        check = FindTag(text, tag, check, recursecompare);
        pos = text.ToLower().IndexOf(tag.ToLower(), check+1);
    }
    return pos;
}
```

代码已经完成。保存并编译解决方案。下次当你尝试保存一个包含指向已删除资源链接的页面时，该链接会被立即移除。试试吧！

下面的截图展示了页面在保存前后的作者视图，该页面包含指向一个已删除资源的链接。在这个例子中，被删除的资源是香蕉树的图片 BananaTree.jpg。超链接和图片都引用了它。

在保存页面之前，已删除的资源项仍然显示在屏幕上：

![](img/442e26bf28f2df81fbf8dff3478f88e3_482_0.png)

保存页面之后，超链接被移除，只留下了文本。图片也被删除：

![](img/442e26bf28f2df81fbf8dff3478f88e3_482_1.png)

- HtmlPlaceholder 的 Html 属性
- ImagePlaceholder 的 Src 属性
- AttachmentPlaceholder 的 Url 属性
- XmlPlaceholder 的 XmlAsString 属性

> 如果你构建了自己自定义的占位符定义对，也需要搜索这些内容。

一旦我们获取了每个占位符的内容，就会用正则表达式将其与字符串 `/NR/rdonlyres/` 进行匹配。如果找到匹配项，则意味着占位符中包含指向资源库中某个资源或本地附件的链接。

为了区分这两者，我们使用 `Searches.GetByGuid()` 方法检索 URL 中存储的 GUID 所引用的对象。如果它不是资源库中的项目，则返回 null 值。否则，我们获取链接指向的资源，并将其路径记录到日志文件中。请将 `ScanPlaceholders()` 方法直接添加到 `WalkTree()` 方法下方。

```csharp
private static void ScanPlaceholders(Posting p, string filePath, CmsApplicationContext cmsContext)
{
    foreach(Placeholder ph in p.Placeholders)
    {
        string html = "";
        if (ph is HtmlPlaceholder)
        {
            html = ((HtmlPlaceholder)ph).Html;
        }
        else if (ph is ImagePlaceholder)
        {
            html = ((ImagePlaceholder)ph).Src;
        }
        else if (ph is AttachmentPlaceholder)
        {
            html = ((AttachmentPlaceholder)ph).Url;
        }
        else if (ph is XmlPlaceholder)
        {
            html = ((XmlPlaceholder)ph).XmlAsString;
        }

        // Find links to resources
        string pattern = @"/NR/rdonlyres/\S+";
        MatchCollection matches;
        matches = Regex.Matches(html, pattern, RegexOptions.IgnoreCase);
        if (matches.Count > 0)
        {
            foreach(Match match in matches)
            {
                //file is either a resource or a local attachment
                string[] arrUrl = match.Value.Split('/');
                string guid = "";
                Resource r;
                if(arrUrl.Length >= 3)
                {
                    guid = "{" + arrUrl[3] + "}";
                    if (IsGuid(guid))
                    {
                        r = cmsContext.Searches.GetByGuid(guid) as Resource;
                        if (r!=null)
                        {
                            WriteToLog(r.Path + "," + p.Path + "," + ph.Name, filePath);
                        }
                    }
                }
            }
        }
    }
}
```

我们使用辅助函数 `IsGuid()` 来测试从 URL 中获取的字符串，判断其是否为有效的 GUID。请将 `IsGuid()` 方法直接添加到 `ScanPlaceholders()` 方法下方。

```csharp
private static bool IsGuid(string guid)
{
    try
    {
        string guidRegEx = "[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-" +
            "[a-fA-F0-9]{4}-[a-fA-F0-9]{12}";
        Regex regex = new Regex(guidRegEx);
        Match m = regex.Match(guid);
        if (m.Success)
        {
            return(true);
        }
        else
        {
            return(false);
        }
    }
    catch
    {
        return (false);
    }
}
```

最后，辅助函数 `WriteToLog()` 会将包含资源信息的一行文本追加到日志文件中。请将其直接添加到 `IsGuid()` 方法下方。

```csharp
private static void WriteToLog(string message, string filePath)
{
    FileStream fs = File.Open(filePath, FileMode.Append);
    StreamWriter sw = new StreamWriter(fs);
    sw.WriteLine(message);

    sw.Flush();
    sw.Close();
    fs.Close();
}
```

代码已经完成。编译解决方案。

> 该解决方案用于查找资源。你可以编写类似的脚本来记录指向频道和帖子的所有链接，示例可见：
> 
> http://www.gotdotnet.com/Community/UserSamples/Details.aspx?SampleGuid=9804F9C9-54D3-4735-9967-41CF1FD88740
> 
> 资源管理者提出的问题，作者和编辑在考虑删除或移动频道和帖子时通常也会提出同样的问题。

## 生成报告

让我们运行脚本，看看生成的报告是什么样子。按 Visual Studio .NET 工具栏上的 F5 键来运行脚本。文件会创建在 FindResources 项目的 `/bin/Debug` 文件夹中。由于这是一个逗号分隔值文件（*.csv），双击它会启动 Microsoft Excel。

![](img/442e26bf28f2df81fbf8dff3478f88e3_488_0.png)

要获取使用了某个特定资源的帖子，只需点击“资源路径”（Resource Path）列，然后从菜单栏中选择“数据 | 排序”（Data | Sort），即可按资源路径对电子表格进行排序。数据排序完成后，按 F5 键搜索你要查找的资源。

![](img/442e26bf28f2df81fbf8dff3478f88e3_488_1.png)

你也可以改为按帖子的路径进行排序，从而查找特定帖子中使用的资源列表。

当然，在电子表格中搜索不如查询数据库方便。除了写入日志文件，你也可以将数据写入数据库表或 XML 文件。你甚至可以在事件处理程序中编写代码，以便每次发布帖子时都更新资源列表。这样一来，你就能为资源管理者提供一个简单的查询界面，让他们实时找到问题的答案。

