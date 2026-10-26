## 提示 #7：如何将不友好的 URL 重写为友好的 URL

如你所知，MCMS 使用了一套内部的“不友好 URL”系统来检索模板文件和发布项。站点的访问者通常不会看到这些不友好的 URL，除非出现以下情况：

- 在“编辑站点”（Edit Site）和“实时站点”（Live Site）视图之间切换时。
- 向服务器执行回发之后。例如，模板文件中可能有一个按钮，用于刷新页面上的某些标签。单击该按钮时，会发生回发，地址栏中的 URL 会从友好 URL 变为不友好版本。
- 站点配置为使用基于唯一 ID 的 URL 时。
- 在框架集中显示页面时。

不友好的 URL 由模板文件的 URL 和追加在其查询字符串中的发布项信息组成：

```
http://localhost/TropicalGreen/Templates/Plant.aspx?NRMODE=Published&NRORIGINALURL=%2fNR%2fexeres%2f0E0047A0-1560-4BA8-859F-C012826391F0%2cframeless%2ehtm%3fNRMODE%3dPublished&FRAMELESS=true&NRNODEGUID=%7b0E0047A0-1560-4BA8-859F-C012826391F0%7d&NRCACHEHINT=ModifyLoggedIn
```

有时，它可能以更简单的形式出现：

```
http://localhost/NR/exeres/0E0047A0-1560-4BA8-859F-C012826391F0,frameless.htm?NRMODE=Published
```

但在后台，MCMS 看到的仍然是复杂的版本。

这两个 URL 实际上都指向同一个页面，例如：

```
http://localhost/tropicalgreen/plantcatalog/coconut.htm
```

但你单凭查看无法分辨出来。

尽管导出频道和发布项能够满足大多数网站的需求，但许多网站会使用与发布项无关的资源。常见的示例包括：网站通过频道渲染脚本提供资源列表，例如资源库中的文档列表。或者，网站某些部分可能包含指向特定资源的链接，例如公司徽标或整个站点范围的帮助文件。此类链接通常出现在用户控件或 Web 窗体中。无论哪种情况，你都需要导出未链接到发布项的资源。

SDAPI 的一个限制是它不能直接导出资源。只有被上次部署之后修改过的发布项所使用的资源才会被导出。站点管理员导出特定资源的唯一方法是使用“站点部署管理器”（Site Deployment Manager）手动创建包。这不仅给管理员增加了额外的工作，他还必须跟踪资源的更新时机。如果管理员未能及时跟上，生产站点将无法及时获得最新信息。

在本节中，我们提出了一种使用 SDAPI 导出资源的解决方案，无论这些资源是否已在发布项中使用过。计划如下：

- 我们知道，资源只有被在上次部署之后修改过的发布项使用时才会被导出。沿着这个思路，我们以编程方式创建一个服务发布项，并在其一个占位符中为所有希望导出的资源创建超链接。
- 由于这些资源现在被用于刚刚创建的发布项中（因此是在上次部署之后修改过的），导出 SDAPI 脚本将创建一个包含该服务发布项及其链接到的所有资源的包。
- 一旦导出包创建完成，该服务发布项就会被删除。

## SDAPIResourceExport 项目

要创建包含资源的导出包并运行导出过程，让我们创建一个 Visual C# 控制台应用程序项目。将项目命名为 SDAPIResourceExport，并存储在磁盘上的合适位置（例如 `c:\`）。将生成的 Class1.cs 文件重命名为 ResourceExport.cs。

我们假设导出过程将在源 MCMS 服务器本身上执行。添加对 COM MSCMS Site Deployment（服务器端）库的引用（注意“MSCMS”中的“S”）。你当然也可以选择在目标服务器上运行该脚本。如果希望这样做，请改为选择 MCMS Site Deployment（客户端）库。

此外，添加对以下 .NET 库的引用（可在 `<install directory>/Microsoft Content Management Server/Server/bin/` 目录中找到）：

- Microsoft.ContentManagement.Common
- Microsoft.ContentManagement.Publishing
- Microsoft.ContentManagement.Publishing.Extensions.Placeholders

接下来，添加解决方案所需的命名空间，如下所示：

```
using System;
using System.Diagnostics;
using Microsoft.ContentManagement.Publishing;
using Microsoft.ContentManagement.Publishing.Extensions.Placeholders;
using SITEDEPLOYSERVERLib;
```





```
bool RecursiveExport = true;

// Create new AppContext in update mode
CmsApplicationContext appContext = new CmsApplicationContext();
appContext.AuthenticateAsCurrentUser(PublishingMode.Update);
```

## 收集资源

接下来，我们获取一个资源库（resource gallery）实例，其中包含我们希望导出的资源。我们编写一个名为 `CollectResources()` 的辅助方法，它返回由资源库中资源超链接组成的 HTML。稍后，我们会将此 HTML 存储到服务发布（service posting）中。

```
static void Main(string[] args)
{
    . . . code continues . . .

    // Get an instance of the resource gallery
    ResourceGallery rsg = appContext.Searches.GetByPath(StartResourceGallery)
                               as ResourceGallery;

    // Start the iteration process
    Console.Write("Collecting resources....");
    string imageString = CollectResources(rsg, RecursiveExport);
    Console.WriteLine("Done.");
}
```

`CollectResources()` 方法接受两个输入参数。第一个是资源库本身，第二个是一个布尔值，指示该方法是否应在子资源库中递归搜索资源。找到的每个资源都会作为超链接添加到 HTML 中。方法结束时，我们将得到一个完整的超链接列表，其中包含我们希望导出的所有资源。

```
static string CollectResources(ResourceGallery gallery, bool recurse)
{
    // The string containing a list of hyperlinks to each resource
    string resString = "";

    // Call this function recursively for each child gallery
    // if the boolean is true
    if (recurse)
    {
        foreach (ResourceGallery rsg in gallery.ResourceGalleries)
        {
            resString += CollectResources(rsg, recurse);
        }
    }

    foreach (Resource item in gallery.Resources)
    {
        resString += "<a href=\"" + item.Url + "\">" + item.Path + "</a><br>";
    }
    return resString;
}
```

## 创建服务发布

现在我们有了包含导出资源列表的 HTML 字符串，需要将其存储到服务发布中。完成此操作的辅助方法是 `CreatePosting()` 方法。它接受 `CmsApplicationContext` 对象和 HTML 字符串值作为输入参数。它返回一个已找到或创建的服务发布实例。

要创建服务发布，需要有一个模板；而要创建模板，首先需要有一个模板库。我们要找的模板直接存储在根模板库下方，其名称由 `AdminTemplateGallery` 类变量指定。在本示例中，这将是一个路径为 `/Templates/_Admin/` 的库。

如果找不到模板库，`CheckAdminTemplate()` 方法会尝试创建它。如果用户没有相应权限，则会抛出异常。如果有充分理由不授予运行脚本的频道管理员在根模板库下创建模板库的权限，你可以事先创建模板库，或将模板库创建在根库以外的其他位置。

找到模板库后，我们查找模板。模板的名称由 `AdminTemplateName` 类变量指定。同样，如果脚本找不到模板，它会尝试创建模板。模板包含一个 `HtmlPlaceholderDefinition`，其名称由 `AdminPhName` 类变量指定。模板必须先提交才能使用。如果由于某种原因模板无法提交，则会抛出异常。

将 `CheckAdminTemplate()` 直接添加到 `CheckChannel()` 方法的下方。

```
static Template CheckAdminTemplate(CmsContext context)
{
    Template t = null;
    TemplateGallery AdminGal = context.RootTemplateGallery
        .TemplateGalleries[AdminTemplateGallery];

    // Get an instance of the admin template
    if (AdminGal != null)
    {
        t = AdminGal.Templates[AdminTemplateName];
    }

    Console.Write("Check Template....");

    // Admin template not found. Create it.
    if (t == null)
    {
        // Get instance of template gallery.
        if (AdminGal == null)
        {
            if (context.RootTemplateGallery.CanCreateTemplateGalleries == false)
            {
                Exception myException = new Exception("Insufficient rights to create "
                    + "AdminTemplateGallery: /Templates/"
                    + AdminTemplateGallery);
                throw myException;
            }
            AdminGal = context.RootTemplateGallery.CreateTemplateGallery();
            if (AdminGal == null)
            {
                Exception myException = new Exception("Failed to create "
                    + "AdminTemplateGallery: /Templates/"+AdminTemplateGallery);
                throw myException;
            }
            AdminGal.Name = AdminTemplateGallery;
            context.CommitAll();
            Console.Write("TemplateGallery created....");
        }
    }

    // Look for admin template within template gallery.
    t = AdminGal.Templates[AdminTemplateName];
```
