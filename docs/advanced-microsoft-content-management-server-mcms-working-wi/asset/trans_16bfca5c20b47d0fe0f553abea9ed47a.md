

## 多个Python框架

Python是一种功能强大且易于学习的编程语言，广泛应用于Web开发、数据科学、人工智能等领域。Python拥有丰富的框架生态系统，每个框架都有其独特的优势和适用场景。

- Django：Python最流行的Web框架之一，遵循“约定优于配置”原则，自带ORM、Admin后台、认证系统等众多内置功能，适合快速构建大型Web应用。
- Flask：一个轻量级的Web框架，核心简单灵活，便于扩展，适合构建小型应用和API服务，常与SQLAlchemy、Jinja2等库配合使用。
- PyTorch：由Facebook开发的深度学习框架，采用动态计算图，调试方便，广受科研人员与开发者的喜爱，适合模型研究和快速原型开发。
- TensorFlow：由Google开发的深度学习框架，支持静态图和动态图，拥有强大的生产部署能力，适用于大规模机器学习和深度学习任务。

选择合适的框架取决于项目的具体需求和开发者的偏好。对于需要快速开发完整功能网站的场景，Django是不错的选择；对于追求轻量化和灵活性的项目，Flask更加适合；而在人工智能领域，PyTorch和TensorFlow则各有所长。

## 在发布模式下隐藏 Web 用户控件

我们希望该控件仅在创作模式下可见。我们首先检查当前的 Web Author Context 模式，判断其是 AuthoringNew 还是 AuthoringReedit。如果是，则显示并启用该控件；否则隐藏该控件。在 Page_Load() 事件处理程序中，添加如下所示的代码。

```
private void Page_Load(object sender, System.EventArgs e)
{
    ... code continues ...
    // Get the current Web Author Context mode
    WebAuthorContextMode webauthorMode = WebAuthorContext.Current.Mode;
    // Set the control to be visible only in authoring modes
    bool displayControl = false;
    if (webauthorMode == WebAuthorContextMode.AuthoringNew
    || webauthorMode == WebAuthorContextMode.AuthoringReedit)
    {
        displayControl = true;
    }
    this.Visible = displayControl;
}
```

## 捕获发布项的保存事件

保存发布项时，我们希望将 TextBox 的内容写入发布项的 DisplayName 属性。难点在于捕获保存事件。好消息是，你可以使用 WebAuthorContext 类的 SavePostingEvent() 事件处理程序。我们只需编写自己的事件（名为 saveDisplayName()，稍后定义），类型为 WebAuthorPostingEventHandler，并将其注册到 SavePostingEvent() 处理程序中。下面的代码演示了如何在 OnInit() 事件处理程序中完成此操作。将突出显示的部分添加到 OnInit() 中。你可能需要展开“Web 窗体设计器生成的代码”区域才能看到它。

```
override protected void OnInit(EventArgs e)
{
    InitializeComponent();
    base.OnInit(e);
    // Register new event being called during save
    WebAuthorContext.Current.SavePostingEvent += new
    WebAuthorPostingEventHandler(saveDisplayName);
}
```

最后，我们编写 saveDisplayName() 方法来保存发布项的显示名称。我们只需将发布项的 DisplayName 属性设置为 TextBox 的内容。

作为 WebAuthorPostingEventHandler 类型的事件，saveDisplayName() 方法接受两个参数：

- `object sender` 包含触发保存的对象。在本例中，sender 是“保存”、“保存并退出”或“保存新页面”按钮，具体取决于作者点击了哪一个。
- `WebAuthorPostingEventArgs e` 包含有关事件的数据。其中最重要的信息是当前正在保存的发布项。

将 saveDisplayName() 直接添加到 OnInit() 事件处理程序的下方。

```
// The event handler that saves the contents of the TextBox to the posting's
// DisplayName property
private void saveDisplayName(object sender, WebAuthorPostingEventArgs e)
{
    e.Posting.DisplayName = txtDisplayName.Text.Trim();
}
```

该控件就完成了。要使用该控件，只需将其拖放到任何需要它的模板文件上。在本示例中，我们选择仅保存发布项的显示名称。你可以轻松扩展该控件，使其处理发布项的所有其他属性和自定义属性。

> 如果你需要在多个项目之间重用代码，使用服务器控件比 Web 用户控件更高效。以下是此类控件的一个示例：
>
> http://www.gotdotnet.com/Community/UserSamples/Details.aspx?SampleGuid=2A85D442-286F-4AD6-8A03-82436BF956A5

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

