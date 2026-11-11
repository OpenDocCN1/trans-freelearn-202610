
```
protected System.Web.UI.WebControls.DataGrid DataGrid1;
private void Page_Load(object sender, System.EventArgs e)
{
    // Get the current CMS Http context
    CmsHttpContext cmsContext = CmsHttpContext.Current;

    // Get a list of all postings that have been modified in the last
    // 7 days
    PostingCollection newPostings = cmsContext.Searches.NewPostings(7);

    // Bind the result to the DataGrid
    DataGrid1.DataSource = newPostings;
    DataGrid1.DataBind();
}
```

为了模块化代码并将与 MCMS PAPI 交互的部分与使用 PAPI 的部分隔离开来，可以创建一个数据抽象层。数据抽象层可以是一个单独的类文件。在开始模块化代码之前，让我们先识别出使用 MCMS PAPI 的部分。如下所示，使用 PAPI 的代码行是获取当前 CmsHttpContext 以及最近七天内修改的帖子列表的部分：

```
protected System.Web.UI.WebControls.DataGrid DataGrid1;
private void Page_Load(object sender, System.EventArgs e)
{
    // Get the current CMS Http context
    CmsHttpContext cmsContext = CmsHttpContext.Current;

    // Get a list of all postings that have been modified in the last
    // 7 days
    PostingCollection newPostings = cmsContext.Searches.NewPostings(7);

    // Bind the result to the DataGrid
    DataGrid1.DataSource = newPostings;
    DataGrid1.DataBind();
}
```

为了隔离代码，我们将提取所有突出显示的代码部分并将其添加到数据抽象层中。在此示例中，数据抽象层只是一个名为 DataAbstractionExample 的单独类文件。这里有两个方法：GetCurrentCMSContext() 获取当前 CmsHttpContext，而 GetListOfPostings() 返回最近七天内修改的所有帖子的列表。

```
public class DataAbstractionExample
{
    private static CmsHttpContext GetCurrentCMSContext()
    {
        return CmsHttpContext.Current;
    }
    public static PostingCollection GetListOfPostings()
    {
        // Get the current CMS Http Context
        CmsHttpContext cmsContext = GetCurrentCMSContext();
        // Get a list of all postings that have been modified in the
        // last 7 days
        PostingCollection newPostings =
            cmsContext.Searches.NewPostings(7);
        return newPostings;
    }
```
```

接下来，我们修改原始代码，使其改为调用数据抽象层中的 GetListOfPostings()。请注意，我们已从 Page_Load() 事件处理程序中移除了所有使用 MCMS PAPI 的代码。

```
    protected System.Web.UI.WebControls.DataGrid DataGrid1;
    private void Page_Load(object sender, System.EventArgs e)
    {
        // Bind the result to the DataGrid
        DataGrid1.DataSource = DataAbstractionExample.GetListOfPostings(7);
        DataGrid1.DataBind();
    }
```
```

乍一看，提取所有与 PAPI 接口的代码似乎不会对网站的行为产生任何影响。事实上，我们所做的只是向应用程序添加了更多代码行，使其变得稍微复杂一些。

尽管如此，将代码分离成模块化层会使迁移过程变得更容易，如果你以后选择进行迁移的话。与其在网站的分散区域进行代码修改，你可以将精力集中在数据抽象层上。在此示例中，我们只需要修改 DataAbstractionExample 类即可，而原始 Web 表单（以及所有使用 GetListOfPostings() 方法的其他 Web 表单）保持不变。

此外，根据你网站的设置，可能可以让 MCMS 和 MOSS 2007 并行运行并分阶段升级网站。这样，你可以计划分段重写数据抽象层。分阶段方法的优势在于，你可以在整个网站升级完成之前就享受到 MOSS 2007 的新功能。

## 使用提供程序模型设计模式

在本書中，我們討論了在構建站點導航控件（第 4 章）或執行窗體身份驗證（第 6 章）時如何使用提供程序模型。

使用提供程序模型扩展了将使用 PAPI 的代码与代码其他部分隔离开来的思想。正如我们在前几章中所看到的，提供程序模型允许我们将“业务逻辑”，它查询 MCMS 仓库，与“表示逻辑”，负责向浏览器返回 HTML 分离。例如，在构建站点导航时，我们使用了标准的 `SiteMapPath` 和 `TreeView` 控件作为表示逻辑。然后我们通过构建 `MCMSsiteMapProvider` 类来实现业务逻辑，该类使用 PAPI 查询仓库以获取导航项列表。

当升级网站时，只需替换提供程序模型即可，要么使用原生的 MOSS 2007 功能，要么使用与 MOSS 通信的替代提供程序。例如，要升级 TropicalGreen 站点的导航控件，我们只需重写 `MCMSsiteMapProvider.cs` 类文件即可。表示层中的 `SiteMapPath` 或 `TreeView` 控件可以无需进一步修改即可重复使用。

> 有关如何规划 MOSS 2007 未来发布的更多详细信息，请参阅以下白皮书：
> http://msdn.microsoft.com/library/en-us/dnmscms02/html/CMSDesigningCMS2002Sol.asp

## 摘要

在本章中，我们讨论了一些有助于使用 MCMS 开发网站的技巧。首先我们看到了如何使用 `PostBackUrl` 属性在页面之间传递数据。随后，我们设计了一种变通方法，使其在 MCMS 网站上正常工作。

接下来，我们讨论了随 ASP.NET 2.0 提供的巧妙的 Ad Rotator 控件。虽然 Ad Rotator 控件已经存在一段时间了，但这个最新版本允许我们将控件的内容直接绑定到非静态 XML 文件。我们现在能够将控件直接与 MCMS 资源库项目集成，为我们的工具箱提供了一个灵活且有用的补充。

最后，我们讨论了一些未来计划的考虑因素，以便于将现有应用程序迁移到下一个版本的 MOSS 2007。

## 索引

A
Active Directory 应用程序模式
  关于 182
  用于身份验证 181
ADAM。参见 Active Directory
  应用程序模式
Ad Rotator
  关于 199
  自定义 Ad Rotator，构建 199, 200
  MCMSAdRotator 199
身份验证控件
  关于 165
  自定义身份验证方案 181
  Login 控件 166
  LoginName 控件 166, 178
  登录页面，创建 175-178
  LoginStatus 控件 166, 178

B
面包屑导航 118

C
级联样式表。另见 样式表；
  另见 主题
  关于 149
  应用 156
  限制 149
频道渲染脚本
  TreeView 控件 139
CMS 虚拟目录
  配置 76, 77
内容页面 89

CorrectCrossPostBack HTTP 模块。参见 HTTP 模块
封面页。参见 摘要页；
跨页回发
  关于 183
  数据，在页面之间传递 184, 185
  HTTP 模块 196
  执行 184
  工作，在 MCMS 网站上 193, 195
CSS。参见 级联样式表
自定义身份验证方案 181
自定义 MCMS 应用程序模板
  示例 MCMS 应用程序 86

D
数据，在页面之间传递 184-186,
  188, 189, 192
数据库
  用于 SQL Server 2005 17, 18

H
HTTP 模块
  关于 196
  创建 196, 198

I
IIS。参见 Internet Information Services
索引页。参见 摘要页；
Internet Information Services
  配置 32
  安装 30, 32
  验证站点配置设置 71

L
Login 控件 166
LoginName 控件
  关于 166
  添加到母版页 180
  使用 179, 180
登录页面
  使用 Login 控件创建 175-178
LoginStatus 控件
  关于 166
  使用 179, 180
M
母版页
  优点 89, 90
  创建，用于 MCMS 93, 94
  LoginName 控件，添加 180
  MCMS 控件，添加 94
  MCMS 模板文件，创建 98
  MCMS 模板，区别 90
  MCMS Web Author 控制台，添加 97, 98
  概述 89
  Page 指令 94
  属性，从模板文件修改 102, 103
  Register 指令，添加 94
  TropicalGreen 设计，使用简单布局重新创建 95, 97
  用途 89
MCMS
  绝对链接，生成 140, 142
  Ad Rotator 199
  身份验证控件 165
  频道名称，映射到主机头 112
  配置 56
  跨页回发 183
  自定义 Ad Rotator 服务器控件 199
  数据库配置应用程序 56
  用于 SQL Server 2005 的数据库 17, 18
  表单身份验证 167
  Internet Explorer Web 控件 45
  隔离 PAPI 代码 203, 205
  母版页，创建 93, 94
  MCMS 2002 先决条件，安装 44
  成员资格提供程序 167
  迁移到 MOSS 2007 202
  导航控件 105
  父节点 113
  发布应用程序编程接口 107
  示例网站 91
  Service Pack 2，安装 26, 52, 54
  Service Pack 2，概述 7
  验证站点配置设置 71
  Site Manager 快捷方式，配置 15, 16
  站点地图提供程序，构建 108, 109
  站点地图提供程序 107
  站点地图 107
  存储过程，修改 26
  样式表 156
  摘要页 142
  带样式表的主题 158, 159
  用户管理系统 166
  Visual Studio 模板 16
MCMS 2002
  Internet Explorer Web 控件 45
  J# 2.0 可再发行组件 44
  先决条件，安装 44
  临时项目，删除 49
MCMS 2002 Service Pack 2
  配置 15
  数据库配置应用程序 56, 58
  开发环境，配置 55
  下载 8
  安装 9, 13, 15, 26, 52, 54
  安装提示 65
  Internet Information Services，配置 32
  Internet Information Services，安装 30, 32
  本地安全策略，配置 27
  概述 7, 8
  服务帐户，创建 28, 29
  Site Manager 快捷方式，配置 15, 16, 55
  SQL Server 2005，配置 40, 42, 43
  SQL Server 2005，安装 33-35, 40
  软件支持 7, 8
  升级 9
