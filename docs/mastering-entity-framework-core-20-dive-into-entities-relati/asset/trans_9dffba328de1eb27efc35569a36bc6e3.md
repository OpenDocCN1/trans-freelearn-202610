

```
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.EntityFrameworkCore.SqlServer.Design
```

我们也可以使用 NuGet 程序包管理器窗口搜索并安装前面列出的程序包，如下所示：

![](img/17e01f90db603e72c0f0c6a7dce8920c_55_0.png)

Microsoft.EntityFrameworkCore.Tools NuGet 程序包

安装 Microsoft.EntityFrameworkCore.Tools 程序包。此程序包将 Entity Framework 命令公开给 NuGet 程序包管理器控制台，我们可以使用它执行诸如脚手架数据库上下文之类的某些操作：

![](img/17e01f90db603e72c0f0c6a7dce8920c_55_1.png)

Microsoft.EntityFrameworkCore.SqlServer.Design NuGet 程序包

安装 `Microsoft.EntityFrameworkCore.SqlServer.Design` 程序包。此程序包让我们可以从 SQL Server 数据库脚手架模型。

> 在撰写本文时，`Microsoft.EntityFrameworkCore.SqlServer.Design 2.0.0` 仅作为预览版发布。

我们已安装了用于在 EF Core 中反向工程现有数据库实体的所需程序包。在下一节中，我们将开始反向工程以将 EF Core 配置为数据库。

## 反向工程数据库

反向工程可以在 **NuGet 程序包管理器**控制台上执行。我们已经了解了如何打开它，因此只需执行以下命令即可脚手架上下文和模型文件：

```
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=MasteringEFCoreDbFirst;Trusted_Connection=True;"
    Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

> 有时我们可能会收到 **The package could not be located** 的错误。解决方法是单独在另一个解决方案中打开项目。如果我们收到 **Unable to open the database** 错误，则在 SQL Management Studio 中提供访问权限（从工作室连接本地）将解决该问题。**SQL Server Management Studio (SSMS)** 是免费版本，可以从 `https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms` 下载。

请参考以下屏幕截图：

![](img/17e01f90db603e72c0f0c6a7dce8920c_57_0.png)

脚手架过程会生成数据库上下文文件和相应的数据模型（在我们的例子中，总共三个文件）。

## 配置数据上下文

自动生成的数据库上下文（在以下代码中呈现）将包括：

- 表/实体的虚拟属性，用于保存相应数据。
- OnConfiguring 方法，它将使用数据库配置 EF。
- OnModelCreating 方法，它将在创建数据库时确保某些约束和关系被构建。在我们的数据库优先方法中它将不会被使用，因为我们已经有了它们。

数据库上下文应包含以下配置：

```
public partial class MasteringEFCoreDbFirstContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            // Move this connection string to config file later
```

```
```csharp
optionsBuilder.UseSqlServer(@"Server=
    (localdb)\mssqllocaldb;Database=MasteringEFCoreDbFirst;
    Trusted_Connection=True;");
}
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Post>(entity =>
    {
        entity.HasIndex(e => e.BlogId).HasName("IX_Post_BlogId");
        entity.Property(e => e.Title).IsRequired();
        entity.HasOne(d => d.Blog).WithMany(p => p.Post)
            .HasForeignKey(d => d.BlogId);
    });
}
```
```

如果您注意到警告，我们需要删除使用依赖注入的部分，这将在 *Registering Context in Services (.NET Core DI)* 部分中执行。

## 使用 Blog 实体

我们在第 1 章 *Kickstart - Introduction to Entity Framework Core* 中详细讨论了 `Blog` 模型，但仍然，以下突出显示的部分对我们来说看起来很新：

- `Blog()` 构造函数初始化 `Post` 属性，这确保集合有一个具体的 HashSet 列表创建并准备好接受任何新项目
- `Post` 属性具有 `virtual` 关键字，这指示 EF 延迟加载导航属性 `Post`：

```
public partial class Blog
{
    public Blog()
    {
        Post = new HashSet<Post>();
    }
    public int Id { get; set; }
    public string Url { get; set; }
    public virtual ICollection<Post> Post { get; set; }
}
```

在 `Blog` 类中没有什么太多可探索的，所以，让我们继续到 `Post` 类。

## 使用 Post 实体

在第 1 章 *Kickstart - Introduction to Entity Framework Core* 中，甚至 `Post` 模型也被详细讨论了，除了我们有一个虚拟的 `Blog` 属性，它只是一个导航属性：

```
public partial class Post
{
    public int Id { get; set; }
    public int BlogId { get; set; }
    public string Content { get; set; }
    public DateTime PublishedDateTime { get; set; }
    public string Title { get; set; }
    public virtual Blog Blog { get; set; }
}
```

我们已经看到了第 1 章 *Kickstart - Introduction to Entity Framework Core* 中手动创建的模型与自动生成的模型之间的差异。让我们看看如何使数据库上下文配置更通用，而不是将连接字符串硬编码在代码中。

## 在服务中注册上下文（.NET Core DI）

`OnConfiguring(DbContextOptionsBuilder optionsBuilder)` 方法中显示的警告需要解决。因此让我们删除该方法（在以下代码中突出显示）并在 `Startup.cs` 文件中使用依赖注入执行配置。

## 重构 OnConfiguring() 方法

如果我们回顾一下我们如何配置数据库上下文，自动生成的代码使用了硬编码的连接字符串用于配置。为了避免它，我们应该有一个机制将数据库上下文选项传递给 DbContext 基类；让我们看看如何操作：

```
public partial class MasteringEFCoreDbFirstContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Move this connection string to config file later
        optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MasteringEFCoreDbFirst;Trusted_Connection=True;");
    }
}
```

还为 MasteringEFCoreDbFirstContext 类包含一个构造函数，该构造函数将通过 Startup 类中的依赖注入初始化 DbContext：

```
public MasteringEFCoreDbFirstContext(
    DbContextOptions<MasteringEFCoreDbFirstContext> options)
    : base(options)
    {
    }
```

我们已经了解了如何将选项传递给数据库上下文基类，现在我们将了解如何使用连接字符串配置选项。

## 重构 ConfigureServices 方法

我们将使用 ConfigureServices() 方法将数据库上下文框架服务包含到服务集合中。添加以下 using 语句以配置 DbContext 选项并将数据库上下文添加到服务列表：

```
using Microsoft.EntityFrameworkCore;
using MasteringEFCore.DatabaseFirst.Final.Models;
```

正如我们在第 1 章 *Kickstart - Introduction to Entity Framework Core* 中所做的那样，我们将上下文配置为服务并将 `DbContext`（使用 `UseSqlServer()` 通过 `DbContextOptionsBuilder` 创建）添加到服务集合：

```
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddDbContext<MasteringEFCoreDbFirstContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString(
        "DefaultConnection")));
    services.AddMvc();
}
```

我们已在框架中配置了数据库上下文，但如果我们仔细观察，会发现连接字符串来自配置。接下来，我们将了解配置如何包含在 `appsettings.json` 中。

## appsettings.json 设置

应用程序设置，如我们之前探索的那样，基于 JSON，并且为了包含一个设置，我们需要添加一个 JSON 键值对。在我们的例子中，`ConnectionStrings` 是键，值又是定义 `DefaultConnection` 的 JSON 对象：

```
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\mssqllocaldb;Database=MasteringEFCoreDbFirst;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  }
}
```

