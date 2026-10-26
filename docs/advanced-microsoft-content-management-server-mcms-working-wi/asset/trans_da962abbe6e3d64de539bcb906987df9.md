
```
deletedObject = cmsContext.Searches.GetByGuid(cmsObjectGuid);
}
// display the path of the object to be deleted
if (!Page.IsPostBack)
{
    litCurrentItem.Text = deletedObject.Path;
}
btnYes.Enabled = deletedObject.CanDelete;
}
```

7. 切换回设计视图，双击 btnYes 按钮。在 btnYes_Click() 事件处理程序中，只需调用 HierarchyItem.Delete() 方法并提交删除操作：

```
private void btnYes_Click(object sender, System.EventArgs e)
{
    try
    {
        deletedObject.Delete();
        cmsContext.CommitAll();
        // display the success message
        lblErrorMessage.Text = "Item deleted successfully!";
    }
    catch(Exception ex)
    {
        // rollback all changes
        cmsContext.RollbackAll();
        // the CMS context needs to be disposed of after a rollback
        cmsContext.Dispose();
        // display error message
        lblErrorMessage.Text = ex.Message;
    }
}
```

如果在处理过程中发生任何错误，删除操作将被回滚，并显示错误消息。

使用 PAPI 删除对象时，需要注意以下几个要点：

-   包含对象的容器只有在其中所有项目都被删除后才能删除。
-   具有现有依赖项的对象只有在所有依赖项被移除后才能删除。例如，如果有基于模板的发布项，则必须先删除这些发布项，才能删除模板。
-   链接到资源的发布项即使资源已被删除，仍会保留到该资源的链接。这是为了防止链接断裂。但是，资源管理器将无法再访问该文件。只有管理员才能查看它们。在第 13 章中，我们将在技巧 #4：如何应对指向已删除资源的链接 中提出处理这些链接的方法。
-   与 Site Manager 中的“删除”选项（删除的对象存储在“已删除项目”回收站中并可恢复）不同，使用 PAPI 删除的对象在删除提交后将无法再访问。MCMS 后台进程将删除所有未被发布项引用的资源。被发布项引用的资源可以通过向 Microsoft 支持部门请求存储过程（参考编号：SOX040517700117）来恢复。

## 总结

在本章中，我们学习了如何使用 PAPI 管理模板库、模板和资源。我们为 CMS Explorer 提供了几个增强功能，包括创建模板库、模板和资源的对话框。我们还展示了如何以编程方式提交（或签入）、复制和移动模板。我们探索的另一个功能是替换现有资源，即可以在保留资源的 GUID 和 URL 的同时替换其文件内容。最后，我们了解了如何使用 PAPI 从内容存储库中删除对象。

正如您所看到的，PAPI 不仅为您提供了构建由 MCMS 管理的网站的基本工具，还允许您对其进行自定义，以满足各种独特的业务需求。

某些搜索引擎使用 META 标签来实现分类法。当页面被索引时，搜索引擎可能会将 Description META 标签的内容添加到名为 description 的字段中。然后，您可以使用搜索引擎的 API 来搜索所有仅在此 description 字段中包含特定关键字的页面。组合多个 META 标签可以实现复杂搜索。例如，您可以编程搜索 description META 标签包含单词“plant”且 location META 标签包含“South America”的页面，以检索所有关于南美洲植物的页面。

请注意，并非每个搜索引擎都支持使用 META 标签，因为它们可能会使用与页面内容无关的信息来影响索引。例如，Google 等引擎有自己内部的机制来为每个页面生成摘要。请参阅您所使用搜索引擎的文档，以查找其识别的 META 标签列表。

大多数搜索引擎能够执行“增量抓取”，即只索引自上次索引以来被修改的页面。执行增量抓取（而不是完全抓取）可以优化索引过程。MCMS 驱动的网站的一个问题是页面内容是动态的，每次访问站点时内容都可能不同。对于搜索索引器，这些网站要么不返回最后修改日期信息，要么总是看起来已被修改。因此，MCMS 站点无法进行增量抓取。在本章中，我们将开发一个解决方案，将最后修改日期输出到 HTTP 响应头中，从而使搜索引擎能够对 MCMS 站点执行增量抓取。

此外，搜索准确性可能会受到页面上的导航控件以及常见的页眉和页脚元素的影响。这可能导致搜索结果中出现过多噪声（例如，如果“新闻稿”页面出现在共享菜单控件中，搜索该页面将产生所有带有此菜单的页面）。通过在站点被索引时隐藏导航元素，可以提高准确性。我们将演示如何实现这一点。

在本章中，我们还将探讨一些技巧和诀窍，以获得最佳、最准确的搜索引擎结果，包括标题、关键字和描述。

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

