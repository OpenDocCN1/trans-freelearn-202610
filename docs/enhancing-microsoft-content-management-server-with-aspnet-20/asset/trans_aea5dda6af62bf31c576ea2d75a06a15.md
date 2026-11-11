
5. 接下来，我们将从 AdRotator 类继承。这将使我们的控件立即具备开箱即用的 Ad Rotator 控件的所有功能。我们将添加一些组件设计值，以便开发人员从工具箱拖放时，控件能获得正确的标签名称。

```
[ToolboxData("<{0}:MCMSAdRotator runat=server></{0}:MCMSAdRotator>")] public class MCMSAdRotator : AdRotator
{
}
```

6. 我们将提供一个名为 AdsGalleryPath 的属性，其中包含包含图像的资源库的路径。将以下代码按所示插入类声明中。

```
public class MCMSAdRotator : AdRotator
{
    // Property to get and set the path of the
    // resource gallery that contains the Ads
    private string adsGalleryPath;

    [Bindable(true)]
    [Category("Behavior")]
    [DefaultValue("")]
    [Localizable(true)]
    public string AdsGalleryPath
    {
        get { return adsGalleryPath; }
        set { adsGalleryPath = value; }
    }
}
```

7. 要显示的广告将从资源库中随机选取。Randomizer() 方法将从 1 到库中项目数量之间随机选取一个数字。在此，我们将使用 Random.Next() 方法生成随机数。我们可以采用更复杂的随机数生成方法，但目前这样就足够了。将以下代码添加到 AdsGalleryPath 属性下方。

```
private int Randomiser(int max)
{
    // Generate a random number
    int index = 0;
    Random rand = new Random();
    index = rand.Next(0, max);

    return index;
}
```

8. 在 Ad Rotator 控件加载但页面加载之前，将调用控件的 AdCreated() 事件。在这里，我们首先获取包含广告的资源库的实例以及构成我们广告的资源集合。我们只检索图像——其他文件格式如 Office 文档将被忽略。之后，我们将调用 Randomiser() 方法来选择要显示的广告。一旦选择了广告，我们将设置控件的属性以显示资源，将其链接到资源描述中指定的 URL，并将图像的工具提示或替代文本设置为资源的显示名称。将 OnAdCreated() 事件处理程序附加到代码中。

```
protected override void OnAdCreated(AdCreatedEventArgs e)
{
    // Get an instance of the resource gallery that contains the Ads
    CmsHttpContext cmsContext = CmsHttpContext.Current;
    ResourceGallery rg =
        (ResourceGallery)cmsContext.Searches.GetByPath(adsGalleryPath);

    if (rg != null)
    {
        // Get only files that are images
        ResourceCollection ads = rg.Resources;
        ads.FilterByMimeType("image", true);

        // Randomly pick one of the Ads
        int resourceIndex = Randomiser(ads.Count);
        Resource r = ads[resourceIndex];

        // Display the image
        e.ImageUrl = r.Url;
        e.NavigateUrl = r.Description;
        e.AlternateText = r.DisplayName;
    }
}
```

9. 控件已完成。保存文件并编译项目。

要查看控件的实际效果，首先通过在工具箱中右键单击并选择“选择项”将其添加到工具箱中。在“选择工具箱项”对话框中，单击“浏览”并选择 TropicalGreenControlLib.dll 文件，通常位于项目的 \bin\debug\ 文件夹中。将控件添加到工具箱后，将控件拖放到 Web 表单或模板文件中的任意位置。将 AdsGalleryPath 属性设置为包含现有资源库项的路径，例如 /Resources/TropicalGreen/PlantCatalog。下次查看页面时，您将看到轮播广告。

## 提示 #3：有关 Microsoft Office SharePoint Server 2007 的注意事项

Web Content Management（WCM）功能目前在 MCMS 中，将成为即将推出的 Microsoft Office SharePoint Server 2007（MOSS 2007）的一部分。MOSS 2007 还将提供更丰富的功能：

- SharePoint 和 MCMS 将作为单一产品发布，从此两者之间不再混淆。除了 WCM 功能外，您还将获得完整的 SharePoint 功能套件，例如：
  - 文档管理
  - 企业搜索
  - 调查和讨论板
  - 列表和提醒
  - 对门户和 Web 部件的内置支持
- 此外，MOSS 2007 还包含多个令人兴奋的新功能：
  - Wiki 和博客。
  - 增强的 Web 作者体验，无需 ActiveX 组件和 Site Manager 的客户端安装。包括节省时间的强大工具，如回收站和一键回滚到以前版本。
  - Site Manager 中的所有功能现在都是 Publishing API 的一部分。例如，您可以以编程方式管理角色、组和用户，以及当前 PAPI 版本中无法完成的任务。
  - 基于 Windows Workflow Foundation 的可扩展工作流。
  - 极强的可扩展性。能够在一个容器中处理数百万个项目。

MOSS 2007 将附带更多新功能和增强功能。如需完整列表，请查看可在 2007 Microsoft Office 系统预览网站下载的 Microsoft Office SharePoint Server 2007 指南（http://www.microsoft.com/office/preview/servers/sharepointserver/highlights.mspx）。

对于我们这些已有 MCMS 安装并计划迁移到 MOSS 2007 的人来说，需要注意的关键点是，Office SharePoint Server 2007 下的 WCM 应用程序将使用与当前 MCMS 版本完全不同的 API 集合。进行大规模代码改造的部分原因是将 MCMS 基础架构与 Windows SharePoint Services（版本 3.0）集成。

虽然 Publishing API 的全面改造可能意味着我们为网站编写的大部分自定义代码都必须重新编码，但好消息是：大多数当前属性和方法的功能将有一对一的映射。详细信息可在文章 Mapping MCMS 2002 APIs to SharePoint 2007（http://msdn.microsoft.com/office/server/moss/2007/migration/default.aspx?pull=/library/en-us/dnmscms02/html/CMSISVersionAPIDiffs.asp）中找到。

此外，Microsoft 将提供迁移实用工具以简化过渡。CMS Assessment Tool 会扫描站点并生成各种报告，例如 PAPI 使用情况汇总报告、PAPI 类使用报告等，帮助您评估使站点为 MOSS 2007 做好准备所需的重编码工作量。CMS Assessment 工具可在 Microsoft 网站下载：http://www.microsoft.com/downloads/details.aspx?FamilyId=360D0E83-FA70-4C24-BCD6-426CAFBCC627&displaylang=en。

出于各种原因，您可能不希望立即迁移到 Microsoft Office SharePoint Server 2007，而选择继续使用 MCMS 2002 构建站点。如果这是您的意图，您可以今天采取措施以便将来更轻松地迁移。您可以：

- 将使用 PAPI 的代码隔离到模块中。
- 在可能的情况下使用 Provider Model 设计模式。

## 将使用 PAPI 的代码隔离到模块中

更改底层 MCMS PAPI 的举措，如果您计划将来升级到 MOSS 2007，将对现有 Web 应用程序产生巨大影响。除非您使用的是没有任何形式定制的开箱即用解决方案，否则很可能您已使用 PAPI 来根据网站需求定制解决方案。

您使用 PAPI 所做的所有定制都必须重写。虽然重写代码不可避免，但您可以规划代码结构，使重新编码过程尽可能轻松。好主意是编写模块化代码并尽可能隔离使用 PAPI 的代码。这样，当升级网站时，需要重写的代码将更容易定位。

例如，一个相当常见的场景是自定义控件，列出站点上最近七天内修改过的所有页面。以前，列表可以编程为在 DataGrid 中显示。列表是从 PAPI 使用 `Searches.NewPostings()` 方法检索的。此外，当页面加载时，列表会直接绑定到 DataGrid，使用 `DataGrid.DataBind()` 方法。以下是代码可能的样子示例：
