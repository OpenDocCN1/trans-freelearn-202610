

`RenderingTemplateCatalogUrl` 属性并指定虚拟路径到你的目录文件。例如，如果你创建了一个名为 `mycatalog.xml` 的新目录，SharePoint 文档占位符在 MCMS 模板文件中的外观如下：

```xml
<cc1:SharePointDocumentPlaceholderControl id="SharePointDocumentPlaceholderControl" runat="server" KeepSynchronized="False" PlaceholderToBind="SPDdocument" RenderingTemplateCatalogUrl="mycatalog.xml">
</cc1:SharePointDocumentPlaceholderControl>
```

## MCMS SharePoint 连接器的局限性

不幸的是，连接器只提供了一种在 MCMS 站点中发布 SharePoint 内容的方式：`SharePointDocumentPlaceholder`，我们刚刚介绍过。但 SharePoint 门户可以包含的内容远不止文档库。SharePoint 中最常用的内容元素之一是列表。SharePoint 随附了许多不同类型的配置列表，例如问题、任务、链接、事件，以及自定义列表。这些列表非常动态、功能强大且可定制，因为它们可以从头构建。连接器最大的缺点之一是它没有提供在 MCMS 网站中发布 SharePoint 列表数据的方法。

能够拥有一个可以显示 SharePoint 列表数据的 MCMS 占位符控件会很有用。由于连接器中未包含此功能，让我们自己构建一个！

我们将继续使用之前的示例，使用我们的 Tropical Green MCMS Internet 站点 http://www.tropicalgreen.net/TropicalGreen/ 和 Tropical Green SharePoint Portal Server Intranet 门户 http://portal.tropicalgreen.net/。假设我们在 SharePoint 门户 Intranet 中有一个事件 Web 部件列表，供员工维护所有与 Tropical Green 相关的事件：

![](img/442e26bf28f2df81fbf8dff3478f88e3_198_0.png)

## 如何向我的事件列表添加项目？

要添加、编辑或查看门户事件列表中的项目，请从门户主页左侧操作菜单中单击管理内容。在文档和列表页面上，单击事件链接。你的列表中可能有也可能没有项目。要添加项目，请单击列表标题中的新建项目链接，输入必要的信息，然后单击表单标题中的保存并关闭链接。重复此过程任意多次。

现在，我们想要做的是在主页上显示所有这些事件。我们将构建一个自定义占位符控件，从 SharePoint 列表中检索这些事件。此自定义占位符控件将添加到现有的 `TropicalGreenControlLib` 项目中，并添加到 `Homepage.aspx` 模板中。`TropicalGreenControlLib` 项目可以在 Tropical Green 下载中找到，并创建于书籍 *Building Websites with Microsoft Content Management Server*（Packt Publishing，2005 年 1 月，ISBN 1-904811-16-7）中。

## 一个用于显示 SharePoint 列表内容的占位符控件

由于我们的占位符控件将检索列表中的所有项目，因此我们需要确定如何访问 SharePoint 门户中的实际列表。一个选项是使用 SharePoint API，但这种方法有一个缺点：我们的 MCMS 站点和 SharePoint 门户必须位于同一服务器上，因为 SharePoint API 只能访问安装它的服务器，而不能访问远程服务器。虽然这在测试环境和某些生产环境中是可行的，但限制很大。另一个选项是通过 SharePoint 随附的 Web 服务访问 SharePoint 列表。列表服务 Web 服务包含返回列表架构、添加、更新和删除项目以及检索列表内容的方法的函数。每个 SharePoint 站点都有对该 Web 服务的引用，因此所有列表都对外部应用程序可见。

通过 Web 服务访问列表比通过 API 访问列表更好。此解决方案可以在同时安装 MCMS 和 SharePoint 的服务器上工作，也可以在两个产品安装在单独服务器上的情况下工作。

我们的占位符控件 `SharePointEventsPlaceholderControl` 将利用 SharePoint 列表服务 Web 服务来检索 SharePoint 事件列表中的项目。在创作模式下，它将要求作者输入 SharePoint 站点的 Web 服务 URL 和事件列表的名称。

让我们开始吧。我们需要做的第一件事是为我们的占位符控件创建一个新类并向项目添加必要的引用：

1. 向 `TropicalGreenControlLib` 项目添加一个新类。将新类命名为 `SharePointEventsPlaceholderControl.cs`。
2. 右键单击项目并选择添加 Web 引用…
3. 在 URL 文本框中指定列表服务 URL，然后单击转到按钮。在我们的例子中，列表服务的地址是 `http://portal.tropicalgreen.net/_vti_bin/lists.asmx`。加载后，通过单击添加引用按钮接受默认的 Web 引用名称。

![](img/442e26bf28f2df81fbf8dff3478f88e3_200_0.png)

现在我们的项目已设置了对 SharePoint 列表服务 Web 服务的引用，并且包含一个空代码文件，其中将包含我们的占位符控件。

我们需要将新类从空壳转换为自定义占位符控件类，通过继承基类并重写必要的方法。为此，我们将添加对一些必要命名空间的引用，继承 BasePlaceholderControl 类，并重写基类中的五个方法（我们不需要为自定义占位符控件添加任何额外引用，因为我们使用的是 `TropicalGreenControlLib` 项目，该项目已包含所有必要的引用，因为它包含其他自定义占位符控件）：

```csharp
using System;
using Microsoft.ContentManagement.Publishing;
using Microsoft.ContentManagement.Publishing.Extensions.Placeholders;
using Microsoft.ContentManagement.WebControls;
using Microsoft.ContentManagement.WebControls.Design;

namespace TropicalGreenControlLib
{
    [SupportedPlaceholderDefinitionType(typeof(XmlPlaceholderDefinition))]
    public class SharePointEventsPlaceholderControl : BasePlaceholderControl
    {
        public SharePointEventsPlaceholderControl()
        {
        }

        protected override void CreateAuthoringChildControls
            (BaseModeContainer authoringContainer)
        {
        }

        protected override void CreatePresentationChildControls
            (BaseModeContainer presentationContainer)
        {
        }

        protected override void LoadPlaceholderContentForAuthoring
            (PlaceholderControlEventArgs e)
        {
        }

        protected override void LoadPlaceholderContentForPresentation
            (PlaceholderControlEventArgs e)
        {
        }

        protected override void SavePlaceholderContent
            (PlaceholderControlSaveEventArgs e)
        {
        }
    }
}
```

我们的空类现在包含了 MCMS 自定义占位符控件的骨架。此时，让我们考虑在发布处于创作模式时，我们需要从用户那里获得哪些输入。此占位符控件的目标是检索 SharePoint 列表的内容。我们已经有了对 SharePoint 列表服务 Web 服务的引用，我们将使用它来检索列表内容，但我们需要知道 Web 服务的确切地址，以防此控件用于访问另一个 SharePoint 站点，而不仅仅是我们的门户。Web 服务包含十二个方法，但我们只对 `GetListItems()` 感兴趣。此方法接受一些参数，但我们唯一需要关心的是 `ListName`（我们将接受其他所有参数的默认值）。

我们需要从用户那里收集以下两个值：

- 包含我们要显示的列表的 SharePoint 站点或门户的 SharePoint 列表服务 Web 服务 URL
- SharePoint 列表的名称

这意味着我们需要两个 `TextBox` 控件，每个输入一个。我们还将添加一个 `LinkButton`，它将使用提供的信息检索列表中的项目并显示事件。最后，因为我们希望控件整齐对齐，我们将使用 HTML 表格来控制布局。

为此，将以下突出显示的代码添加到代码文件顶部：

```csharp
using System;
using System.Collections;
using System.Net;
using System.Web.UI;
using System.Web.UI.HtmlControls;
using System.Web.UI.WebControls;
using System.Xml;
using Microsoft.ContentManagement.Publishing;
using Microsoft.ContentManagement.Publishing.Extensions.Placeholders;
using Microsoft.ContentManagement.WebControls;
using Microsoft.ContentManagement.WebControls.Design;
```

```csharp
namespace TropicalGreenControlLib
{
    [SupportedPlaceholderDefinitionType(typeof(XmlPlaceholderDefinition))]
    public class SharePointEventsPlaceholderControl : BasePlaceholderControl
```

