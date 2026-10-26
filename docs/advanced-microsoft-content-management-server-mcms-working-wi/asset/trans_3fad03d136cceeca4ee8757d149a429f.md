

## 必备操作、提示和技巧

- 使用 `WebAuthorContext.SavePostingEvent()` 事件在每个模板文件中注册清理已删除资源链接的方法。但是，这样做意味着需要在解决方案中的所有模板文件上重复代码。
- 在 `global.asax` 文件的 `CmsPosting_PlaceholderPropertyChanging()` 事件处理程序中添加清理死链所需的逻辑。

虽然这两种方案都可行，但让我们选择将清理代码放在 `global.asax` 文件的 `CmsPosting_PlaceholderPropertyChanging()` 事件处理程序中。这样可以确保只要占位符内容发生变化，就会触发清理过程。可以保证在资源被删除后保存的页面上不会存在指向已删除资源的链接。

首先，让我们注册 `CmsPosting_PlaceholderPropertyChanging()` 事件处理程序。

1. 打开 `global.asax` 文件的代码隐藏文件（`global.asax.cs`）。
2. 添加以下命名空间：

```csharp
using Microsoft.ContentManagement.Publishing;
using Microsoft.ContentManagement.Publishing.Events;
using Microsoft.ContentManagement.Publishing.Extensions.Placeholders;
```

3. 在 `Global()` 构造函数下方，添加 `CmsPosting_PlaceholderPropertyChanging()` 事件处理程序：

```csharp
protected void CmsPosting_PlaceholderPropertyChanging( Object sender,
    PropertyChangingEventArgs e )
{
}
```

资源通常链接在三个可能的位置。它们可能被用于：

- 作为图像的 `ImagePlaceholder`。资源链接存储在 `Src` 属性值中。
- 作为附件的 `AttachmentPlaceholder`。资源链接存储在 `Url` 属性值中。
- 作为图像或附件的 `HtmlPlaceholder`。它们混合在从 `Html` 属性值检索到的内容中。

> 资源链接也可以在 XmlPlaceholders 中找到。根据您特定的 XML 架构，也请考虑在解决方案中处理它们。

以下代码依次处理这些情况。将代码添加到 `global.asax.cs` 文件中的 `CmsPosting_PlaceholderPropertyChanging()` 事件处理程序内。

```csharp
public void CmsPosting_PlaceholderPropertyChanging( Object sender,
    PropertyChangingEventArgs e )
{
    string propName = e.PropertyName;
    string propValue = e.PropertyValue as string;
    switch (propName)
    {
        case "Src": // handle ImagePlaceholder
        case "Url": // handle AttachmentPlaceholder
        {
            propValue = HandleImgAndAttPlaceholderTypes( propValue );
            break;
        }
        case "Html": // handle HtmlPlaceholder
        {
            propValue = HandleImageTags( propValue );
            propValue = HandleAnchorTags( propValue );
            break;
        }
    }
    e.PropertyValue = propValue;
}
```

```csharp
private bool IsResourceDeleted(string link)
{
    // Extract Resource GUID from URL
    string[] split = link.Split('/');
    string GUID = split[3];

    // Create new context with admin rights to be able to see deleted items
    CmsApplicationContext appContext = new CmsApplicationContext();
    appContext.AuthenticateAsUser("WinNT://server/cmsAdmin",
                                 "cmsAdminPassword",
                                 PublishingMode.Unpublished);

    Resource r = appContext.Searches.GetByGuid("{"+GUID+"}") as Resource;

    // Check to see if it belongs to the Archive Folder
    bool isDeleted = false;
    if (r != null)
    {
        // Resource has been deleted
        isDeleted = r.Path.StartsWith("/Archive Folder");
    }
    else
    {
        // Unable to retrieve resource.
        // Possibly because the resource was deleted using Site Manager
        isDeleted = true;
    }

    // Dispose admin context
    appContext.Dispose();
    appContext = null;

    // Return result
    return isDeleted;
}
```

现在我们知道如何识别指向已删除资源的链接，我们可以删除它们！让我们先从 ImagePlaceholders 和 AttachmentPlaceholders 中的链接开始。

> 在上面的示例中，我们为了简单起见，将管理员的用户 ID 和密码硬编码到代码中。最好将其存储在 web.config 文件中并对密码进行加密。或者，将用户 ID 和密码存储在另一个文件夹中的文件中，并使用 Windows 安全措施对其进行保护。

### 从 ImagePlaceholders 和 AttachmentPlaceholders 中删除链接

从 ImagePlaceholders 和 AttachmentPlaceholders 中删除链接相对简单。我们首先检查 ImagePlaceholder.Src 和 AttachmentPlaceholder.Url 属性的值，看看它们是否包含指向已删除资源的链接，通过运行它们的值通过 IsResourceDeleted() 方法。如果链接确实指向已删除的资源，我们将 ImagePlaceholder.Src 和 AttachmentPlaceholder.Url 值设置为空字符串。

下面的 HandleImgAndAttPlaceholderTypes() 方法展示了如何操作。指向内部资源的链接始终以 /nr/rdonlyres/ 开头，因此我们只需要考虑以它开头的链接。将以下代码直接添加到 IsResourceDeleted() 方法下方。

```csharp
private string HandleImgAndAttPlaceholderTypes(string propValue)
{
    if (propValue.ToLower().StartsWith("/nr/rdonlyres"))
    {
        if (IsResourceDeleted(propValue))
        {
            propValue = "";
        }
    }
    return propValue;
}
```

### 从 HtmlPlaceholders 中删除链接

由于 HtmlPlaceholders 包含附件和图像混合在一堆 HTML 标签中，因此从中删除死链稍微复杂一些。

#### 处理图像

让我们先处理图像。图像通过图像标签 `<img>` 链接到 HTML 文件。图像标签通常包含多个属性，如 src、border、alt 等。以下是 `<img>` 标签的示例：

```html
<img src="/NR/rdonlyres/8C78AC41-591D-4626-AFC8-19AC8375914E/0/myresource.gif">
```

获取整个标签的一个好方法是搜索起始 `<img` 字符串和结束的 >。我们检查它是否链接到 MCMS 资源。如果是，我们将其通过之前定义的 **IsResourceDeleted()** 方法运行。显示已删除资源库项目的图像会通过删除整个 `<img>` 标签来删除。

HandleImageTags() 方法执行此工作。将 HandleImageTags() 直接添加到 HandleImgAndAttPlaceholderTypes() 方法下方。

```csharp
private string HandleImageTags(string propValue)
{
    // Look for <img> tags within the content
    int start = propValue.ToLower().IndexOf("<img ");

    // Found an <img> tag.
    while (start >= 0)
    {
        // Get the closing angle bracket of the tag
        int end = propValue.IndexOf(">", start);
        if (end > start)
        {
            // Get the entire <img> tag
            string tag = propValue.Substring(start,end-start+1);

            // Get the link to the resource gallery item
            int linkStart = tag.ToLower().IndexOf("\"/nr/rdonlyres");
            if (linkStart >= 0)
            {
                int linkEnd = tag.IndexOf("\"", linkStart+1);
                if (linkEnd > linkStart)
                {
                    string linkStr = tag.Substring(linkStart, linkEnd-linkStart+1);

                    // Check to see if the link points to the deleted resource
                    if (IsResourceDeleted(linkStr))
                    {
                        // Resource has been deleted. Remove the tag entirely
                        propValue = propValue.Replace(tag,"");
                        tag = "";
                    }
                }
            }
        }
        // Look for more <img> tags
        start = propValue.ToLower().IndexOf("<img ", start+tag.Length);
    }
    return propValue;
}
```

#### 处理附件

处理指向已删除资源的附件与处理图像类似，只有一个主要区别。对于附件，我们处理的不只是一个，而是成对的锚标签：<a> 和 </a>。以下是 HTML 中锚标签的示例：

```html
<a href="/NR/rdonlyres/8C78AC41-591D-4626-AFC8-19AC8375914E/0/myresource.gif">
    My Resource
</a>
```

要增加复杂性，锚标签可能是嵌套的。

我们将像之前一样开始，通过查找起始 <a> 和结束 </a> 标签。一旦我们得到整个锚元素，我们检查它是否包含指向 MCMS 资源的链接。如果包含，我们使用 IsResourceDeleted() 方法确定资源是否已被删除。

执行此操作的代码是 HandleAnchorTags() 方法。将它直接添加到 HandleImageTags() 方法下方。如果附件指向已删除的资源，我们会使用稍后描述的 RemoveTagPair() 方法删除 <a></a> 标签对，但保留它们之间的内容不变。

```csharp
private string HandleAnchorTags(string propValue)
{
    // Get the index of the starting <a tag
    int start = propValue.ToLower().IndexOf("<a ");
    while (start >= 0)
    {
        // Get the closing angle bracket of the opening <a> tag
        int end = propValue.IndexOf(">",start);
        if (end > start)
        {
            string tag = propValue.Substring(start, end-start+1);
            int linkStart = tag.ToLower().IndexOf("\"/nr/rdonlyres");
            if (linkStart >= 0)
            {
                int linkEnd = tag.IndexOf("\"", linkStart+1);
                if (linkEnd > linkStart)
                {
                    string linkStr = tag.Substring(linkStart, linkEnd-linkStart+1);
                    if (IsResourceDeleted(linkStr))
                    {
                        propValue = RemoveTagPair(propValue, tag, "</a>", "<a");
                        tag = "";
                    }
                }
            }
            start = propValue.ToLower().IndexOf("<a ", start+tag.Length);
        }
    }
    return propValue;
}
```

