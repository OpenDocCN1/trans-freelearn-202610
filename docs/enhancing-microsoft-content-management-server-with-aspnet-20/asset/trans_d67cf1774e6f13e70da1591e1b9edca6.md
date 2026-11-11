

为了解决此问题，我们需要修改 MCMS 添加的客户端脚本的行为，在执行跨页回发时将 __CMS_PostbackFormBeenReset 设置为 true。以下是可能的做法。

- 首先，我们将确定是否需要跨页回发。为此，我们查找名为 WebForm_DoPostBackWithOptions() 的方法。WebForm_DoPostBackWithOptions() 方法是对旧版 ASP.NET 1.x __doPostBack() 方法的增强，并在页面上至少有一个按钮执行跨页回发时由 ASP.NET 注入。
- 接下来，我们复制原始的 WebForm_DoPostBackWithOptions() 方法，并将其存储在名为 __CrossPostBack_Backup_WebForm_DoPostBackWithOptions 的变量中。
- 随后，我们将 WebForm_DoPostBackWithOptions() 分配一个新的方法，名为 __CrossPostBack_Redirect_WebForm_DoPostBackWithOptions()。
- __CrossPostBack_Redirect_WebForm_DoPostBackWithOptions() 方法 simply 设置 __CMS_PostbackFormBeenReset 为 true 并调用原始的 WebForm_DoPostBackWithOptions() 方法，该方法已备份在 __CrossPostBack_Backup_WebForm_DoPostBackWithOptions 变量中。

现在不必担心输入代码。我们将在下一节中使用服务器端代码将代码注入到所有页面。

```
if ( typeof WebForm_DoPostBackWithOptions != "undefined" )
{
    __CrossPostBack_Backup_WebForm_DoPostBackWithOptions =
        WebForm_DoPostBackWithOptions;

    WebForm_DoPostBackWithOptions =
        __CrossPostBack_Redirect_WebForm_DoPostBackWithOptions;
}

function __CrossPostBack_Redirect_WebForm_DoPostBackWithOptions(options)
{
    __CMS_PostbackFormBeenReset = true;
    __CrossPostBack_Backup_WebForm_DoPostBackWithOptions(options);
}
```

## CorrectCrossPostBack HTTP 模块

由于对客户端脚本的修改可能会影响解决方案中的所有模板文件，实现此更改的一个好方法就是编写一个 HTTP 模块。HTTP 模块会拦截所有页面请求，并且可以编程为自动将修改后的脚本注入到需要它们的模板文件中。

要创建 HTTP 模块，首先检查解决方案中是否存在 TropicalGreenHttpModules 类库。如果不存在，请将其添加为新的 Visual C# 类库。

接下来，在 TropicalGreenHttpModules 类库中添加一个名为 CorrectCrossPostBack.cs 的类。在类声明上方，添加此处突出显示的命名空间：

```
. . . code continues . . .

using System.Web;
using Microsoft.ContentManagement.Publishing;

namespace TropicalGreenHttpModules
{
    . . . code continues . . .
}
```

由于我们正在构建一个 HTTP 模块，让该类实现 IHttpModule 接口。

```
class CorrectCrossPostBack : IHttpModule
{
}
```

IHttpModule 接口要求类实现 Init() 和 Dispose() 方法。我们在 Dispose() 方法中无需执行任何操作。

在 Init() 方法中，我们将为 HTTP 模块的 PreRequestHandlerExecute 事件注册一个新的事件处理程序，名为 OnPreRequestHandlerExecute()。将这两个方法追加到类文件中。

```
public void Dispose()
{
    // Nothing to do.
}

public void Init(HttpApplication httpApp)
{
```

```
httpApp.PreRequestHandlerExecute += new EventHandler
    (this.OnPreRequestHandlerExecute);
}
```

接下来，我们将添加 OnPreRequestHandlerExecute() 事件处理程序，该处理程序检查当前请求是否为 ASP（或 ASP.NET）页面。为此，它检查处理程序的类型是否以 ASP 开头。HTTP 模块将忽略所有其他请求，例如 ASP.NET 2.0 资源处理程序或其他自定义处理程序的请求。

在 OnPreRequestHandlerExecute() 事件处理程序内部，我们将进行另一项检查，以查看请求是否为 MCMS 频道项。如果是，我们将注册另一个事件处理程序 OnInit()。

```
public void OnPreRequestHandlerExecute(object sender, EventArgs e)
{
    HttpContext ctx = ((HttpApplication)sender).Context;
    IHttpHandler handler = ctx.Handler;

    // Only execute for ASP.NET handler
    // not for custom http handlers or for the ASP.NET 2.0 resource
    // handlers.
    if (handler.GetType().ToString().StartsWith("ASP."))
    {
        try
        {
            if (CmsHttpContext.Current.ChannelItem != null)
            {
                ((System.Web.UI.Page)handler).Init += new
                    EventHandler(this.OnInit);
            }
        }
        catch
        {
            // This will happen if the request is in the middle of an
            // expired forms authentication login we just ignore this.
        }
    }
}
```

最后，我们将注入纠正跨页回发问题的客户端脚本。我们将构建脚本，并在 OnInit() 方法中使用 Page.ClientScript.RegisterStartupScript() 方法。

```
string Override_WebForm_DoPostBackWithOptions =
    "    if ( typeof WebForm_DoPostBackWithOptions != \"undefined\" ) \\n" +
```

```
"  {\n" +
"    __CrossPostBack_Backup_WebForm_DoPostBackWithOptions = " + 
"        WebForm_DoPostBackWithOptions;\n" + 
"    WebForm_DoPostBackWithOptions = " + 
"        __CrossPostBack_Redirect_WebForm_DoPostBackWithOptions;\n" + 
"  }\n" + 
"  function __CrossPostBack_Redirect_WebForm_DoPostBackWithOption\ns(options)" + 
"  {\n" + 
"    __CMS_PostbackFormBeenReset = true;\n" + 
"    __CrossPostBack_Backup_WebForm_DoPostBackWithOptions\n    (options);\n" + 
"  }\n";
```

```csharp
public void OnInit(object sender, EventArgs eventArgs)
{
    System.Web.UI.Page currentPage = sender as System.Web.UI.Page;
    currentPage.ClientScript.RegisterStartupScript(this.GetType(),
        "Override_WebForm_DoPostBackWithOptions",
        Override_WebForm_DoPostBackWithOptions, true);
}
```

该类已完成。保存并构建项目。

要使用 HTTP 模块，请在 web.config 项目的 <HttpModules> 部分添加以下条目。

```xml
<add type="TropicalGreenHttpModules.CorrectCrossPostBack,
    TropicalGreenHttpModules"
    name="CorrectCrossPostBack"/>
```

现在，让我们看看 HTTP 模块是否确实纠正了问题。尝试从订购页面订购一株植物。当单击 Order 按钮时，注意信息已正确发布到确认页面。

## 提示 #2：如何实现广告轮播

如今广告无处不在。在大多数商业互联网网站上，您会发现某种形式的广告，要么是沿着页面高度或宽度运行的横幅，要么是每次页面加载时吸引注意力的弹出窗口。

热门网站可能有多家广告商要求在线投放广告。当竞争激烈时，必须制定一个流程来确定谁的广告先出现、广告应如何排序以及何时发布或下线。

好消息是：ASP.NET 2.0 附带了 Ad Rotator 控件。在本提示中，我们将了解 Ad Rotator 如何与 MCMS 资源库项目集成。使用 MCMS 内置的资源管理器，用户可以添加新图像或替换现有图像，而不必担心超链接损坏。当图像从库中删除时，它会立即下线。

虽然 Ad Rotator 并不是一个新控件，自 ASP.NET 1.x 时代（甚至更早）就已存在，但旧版本的控件要求开发人员在物理 XML 文件中定义广告列表。控件的增强版本允许以编程方式填充广告及其属性，使其非常适合与 MCMS 集成。

## MCMSAdRotator

让我们构建一个自定义 Ad Rotator 服务器控件。完成后，该控件的行为与开箱即用的 Ad Rotator 控件完全相同，但会与选定的资源库集成。它将从库中随机选择一个资源并显示图像。刷新页面时，广告会发生变化！

- 1. 首先，确保 TropicalGreen 解决方案包含一个名为 `TropicalGreenControlLib` 的 Visual C# 类库项目。否则，请创建它。
- 2. 项目应添加以下引用：
  - `System.Web`
  - `Microsoft.ContentManagement.Publishing`
- 3. 向 **TropicalGreenControlLib** 项目添加一个类文件。将类命名为 **MCMSAdRotator.cs**。
- 4. 在命名空间声明上方添加突出显示的命名空间。

```csharp
using System;
using System.Collections.Generic;
using System.Text;

using System.ComponentModel;
using System.Web.UI;
using System.Web.UI.WebControls;
using Microsoft.ContentManagement.Publishing;

namespace TropicalGreenControlLib
{
    . . . code continues . . .
}
```
