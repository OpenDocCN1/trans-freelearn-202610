
2. 在项目模板中搜索“ASP.NET Core Web 应用程序”并选择它。如下面的截图所示，在“名称”中输入 MasteringEFCore.Web，在“解决方案名称”中输入 MasteringEFCore，然后点击“确定”：

![](img/17e01f90db603e72c0f0c6a7dce8920c_26_0.png)

新项目

## 文件菜单

从文件菜单中，执行以下步骤：

1. 选择“新建”|“项目”。
2. 在左侧窗格中，选择“模板”|“Visual C#”|“.NET Core”。
3. 从列表中选择“ASP.NET Core Web 应用程序”模板。
4. 如上图所示，在 **名称** 中输入 `MasteringEFCore.CodeFirst.Starter`，在 **解决方案名称** 中输入 `MasteringEFCore`，然后点击 **确定**。

无论采用上述哪种方式，所选模板都会提供 **“新建 ASP.NET Web 应用程序 (.NET Core)”** 对话框，让我们从以下选项中选择：

- **空**
- **Web API**：创建一个 Web API 项目
- **Web 应用程序（模型-视图-控制器）**：创建一个 MVC Web 应用程序，也允许我们创建 API

我们将从对话框中选择 **Web 应用程序（模型-视图-控制器）**，如下所示：

![](img/17e01f90db603e72c0f0c6a7dce8920c_27_0.png)

新建 ASP.NET Web 项目对话框

5. 在我们的案例中，选择 **.NET Core**、**ASP.NET Core 2.0** 和 **Web 应用程序（模型-视图-控制器）** 模板，同时将身份验证设置为 **无身份验证**。点击“确定”：

![](img/17e01f90db603e72c0f0c6a7dce8920c_28_0.png)

生成的 Web 应用程序显示了一个对我们来说全新的选项卡式界面（而不是显示 `index.cshtml`）。它允许我们直接从起始页访问文档、连接任何服务，甚至决定发布选项。

> 💡 **提示**
>
> 如果仔细观察，你会注意到 Visual Studio 正在静默还原包；在 .NET Core 中，几乎所有东西都是某个包的一部分。不再有即使我们不需要也会加载大量 DLL 的重量级框架了！现在一切都被拆分为更轻量的包，我们可以根据需求来使用它们。

我知道深入讲解 MVC 可能稍微超出本章的范围，但在深入探讨 Entity Framework 之前，让我们先了解一些细节。

## Web 应用的结构

一个 .NET Core Web 应用程序由以下文件夹组成：

- `Dependencies`：SDK、服务器和客户端依赖项
- `wwwroot`：所有静态资源都应放在这里
- `Connected Services`：用于连接 Marketplace 中提供的外部服务
- `launchSettings.json`：启动 Web 应用程序所需的设置
- `appSettings.json`：配置，例如日志记录和连接字符串
- `bower.json`：客户端依赖项应在此处配置
- `bundleConfig.json`：捆绑功能现已移至 JSON 配置中
- `Program.cs`：一切都从 `Main()` 开始，任何程序都可以使用 WebHostBuilder API 变成 Web 应用程序
- `Startup.cs`：用于添加和配置启动服务，如 MVC 支持、日志记录、静态文件支持等
- `Controllers, Views`：属于 MVC 的一部分，包含操作和对应的视图

到目前为止我们讨论的结构如下图所示：

![](img/17e01f90db603e72c0f0c6a7dce8920c_29_0.png)

在 Views\Shared\_Layout.cshtml 中，下面突出显示的部分应修改为所需的应用程序名称：

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,
    initial-scale=1.0" />
<title>@ViewData["Title"] - MasteringEF Core.Web</title>
...
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top">
<div class="container">
<div class="navbar-header">
...
<a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">MasteringEF Core.Web</a>
...
<div class="container body-content">
...
<footer>
<p>&copy; 2017 - MasteringEF Core.Web</p>
</footer>
...
</body>
```

我们已经创建了一个不带身份验证的 .NET Core Web 应用程序，并探索了项目结构，这可能有助于我们理解 .NET Core 中的 MVC。如果展开依赖项，就会明显发现我们没有对 Entity Framework (EF) Core 的内置支持。我们将介绍识别和安装包的几种不同方法。

## 安装 Entity Framework

Entity Framework 包应作为 NuGet 包的一部分安装，可通过以下方式完成：

1. 转到“包管理器控制台”（工具 | NuGet 包管理器 | 包管理器控制台），选择要安装包的项目：

   在 PM 控制台中添加以下命令，以在所选项目上安装包：

   ```
   Install-Package Microsoft.EntityFrameworkCore.SqlServer
   ```

   包管理器控制台将打开，如下面的截图所示，请在此处使用上述命令安装包：

   ![](img/17e01f90db603e72c0f0c6a7dce8920c_31_0.png)

2. 转到“包管理”选项卡（可以从“工具”或“依赖项/项目”中进入）。
   - 若要为整个解决方案安装包，并使其对该解决方案中的所有项目可用，请转到“工具”|“NuGet 包管理器”|“管理解决方案的 NuGet 包...”，或在解决方案资源管理器中右键单击解决方案，然后选择“管理解决方案的 NuGet 包...”。
   - 若要按项目安装包，请右键单击目标项目中的“依赖项”，或右键单击目标项目，然后选择“管理 NuGet 包...”。

3. 搜索 Microsoft.EntityFrameworkCore.SqlServer，选择稳定版本 2.0.0 并安装该包。它也包含所有依赖包（关键依赖项，如 System.Data.SqlClient 和 Microsoft.EntityFrameworkCore.Relational）：

   ![](img/17e01f90db603e72c0f0c6a7dce8920c_32_0.png)

NuGet 包管理器窗口

到目前为止，我们已经了解了使用包管理器控制台的不同方式，并安装了与 EF Core 相关的包。在下一节中，我们将开始构建架构，然后使用 EF 来处理所创建的实体。

## 数据模型

在 .NET 世界中，如果我们在创建数据库之前很久就开始创建数据模型，那么我们有点偏离传统做法。没错，这被广泛称为 **Code-First（代码优先）方法**。让我们使用 code-first 为 Blogging 应用程序创建实体类，并将它们放入项目下的 Models 文件夹中。

### Blog 实体

创建一个 `Blog.cs` 类文件，并包含以下属性：

```
public class Blog
{
    public int Id { get; set; }
    public string Url { get; set; }
    public ICollection<Post> Posts { get; set; }
}
```

Entity Framework 会查找任何名为 Id 或 TypeNameId 的属性，并将它们标记为表的主键。Posts 属性是一个导航属性，包含与此 Blog 实体相关的 Post 项。对于导航属性，我们使用 `ICollection<T>` 还是 `IEnumerable<T>` 并不重要，EF 会为我们创建一个集合，默认是 `HashSet<T>`。我们也可以使用 `List<T>` 创建具体集合。

### Post 实体

创建一个 `Post.cs` 类文件，并包含以下属性：

```
public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PublishedDateTime { get; set; }
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

`BlogId` 属性是为相应的 `Blog` 导航属性创建的外键。你可能已经注意到，在这种情况下，我们有一个单项作为导航属性，而不是 `Blog` 实体中的列表。这就是关系类型发挥作用的地方，我们将在第 3 章 *关系 – 术语和约定* 中进行更深入的探讨。

![](img/17e01f90db603e72c0f0c6a7dce8920c_34_0.png)

EF 允许我们在实体中创建没有外键的单个导航属性。在这些情况下，EF 将使用 `Blog-Id` 模式（`Blog` 导航属性及其 `Id` 主键）在数据库表中为我们创建外键。EF 会针对 `Id` 主键自动为所有导航属性生成外键，但也允许我们以不同方式命名，并通过自定义特性对其进行修饰。

到目前为止，我们已经构建了应用程序所需的架构，但尚未在 EF 中进行配置，因此让我们看看如何使用数据库上下文将数据模型与 EF 连接/配置。

### 数据库上下文

EF 的主要入口点是任何继承 `Microsoft.EntityFrameworkCore.DbContext` 类的类。让我们创建一个名为 `BlogContext` 的类并继承该类。我们将把上下文和其他 EF 相关配置放在 `Data` 文件夹中。在项目中创建一个 `Data` 文件夹，并在该文件夹中创建 `BlogContext.cs`：

```
public class BlogContext: DbContext
{
    public BlogContext(DbContextOptions<BlogContext> options)
        : base(options)
    {
    }

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

EF 将 `DbSet<T>` 解释为数据库表；我们已经为博客系统的所有实体创建了 `DbSet<T>` 属性。我们通常以复数形式命名这些属性，因为该属性将持有实体列表，并且 EF 在数据库中创建表时将使用这些属性名。

