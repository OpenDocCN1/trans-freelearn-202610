

**删除**页面提供了一个确认视图，让用户可以确认是否要删除该项：

![](img/17e01f90db603e72c0f0c6a7dce8920c_47_0.png)

当用户在列表页面的项目行中选择**删除**超链接时，将显示此**删除**页面。我们不会直接从操作中删除博客，而是将用户路由到**删除**页面，在执行操作前获取确认。

我们已经了解了如何使用 EF Core 执行 CRUD 操作。由于探索 MVC 超出了本书的范围，我们只坚持分析与 EF 相关的脚手架。

### 小结

我们从实体框架开始，首先从高层次了解了它与传统方法相比有何不同。我们还研究了如何构建 .NET 环境，以及如何使用实体框架创建和配置 .NET Core Web 应用程序。我们探索了 NuGet 包和包管理器，它们将在整本书中被广泛使用。在本章中，我们还确定并安装了实体框架所需的包。使用 Code-First 方法，我们构建了架构，使用 EF 对其进行了配置，并使用架构和种子数据创建和填充了数据库。最后，我们使用脚手架工具（在过程中安装）在 MVC 应用程序中使用了构建的架构，并查看了控制器中数据库上下文的使用。Code-First 方法可用于构建新系统，但对于现有系统，我们需要不同的方法。这就是**Database-First**方法的用武之地。让我们在*第 2 章*《另一种方式——数据库优先方法》中探讨这一点。

## 另一种方式——数据库优先方法

在第 1 章《快速入门——实体框架核心简介》中，我们接触了**实体框架（EF）**的 Code-First 方法，但这种方法并非在所有场景中都有用。我们需要一种使用 EF Core 逆向工程现有数据库的机制，它指导我们执行迁移，并帮助我们保留模型更改，这些更改在迁移过程中不会丢失。

当 EF 首次发布时，它只支持数据库优先方法，这意味着我们只能在现有数据库上使用该框架。与前一章讨论的 Code-First 方法不同，当我们决定在现有系统中使用 EF 时，数据库优先方法将被广泛使用。

我们在这里将涵盖的主题有：

- 准备数据库
- 创建新项目
- 安装实体框架
- 逆向工程数据库
- 在服务中注册上下文（.NET Core DI）
- 执行 CRUD 操作

### 准备数据库

我们将使用*第 1 章《快速入门——实体框架核心简介》*中使用的同一博客系统。在这种情况下，我们将为现有数据库创建所需的 SQL 查询，然后使用数据库优先方法构建我们的博客系统。让我们编写博客系统所需的 `Blog` 和 `Post` 的 SQL 查询。

#### Blog 实体脚本

我们将创建 `Blog` 表，然后修改它以添加主键约束，最后向表中插入一些示例数据。完整的脚本可在 GitHub 仓库中获得，地址为 `https://github.com/PacktPublishing/Mastering-Entity-Framework-Core/blob/master/Chapter%202/Final/MasteringEFCore.DatabaseFirst.Final/dbo.Blog.sql`。

创建 `Blog` 表并插入数据所需的脚本如下所示：

```sql
// 为简洁起见，代码已移除
CREATE TABLE [dbo].[Blog] (
    [Id] INT            IDENTITY (1, 1) NOT NULL,
    [Url] NVARCHAR (MAX) NULL
);
GO
// 为简洁起见，代码已移除
INSERT INTO [Blog] (Url) VALUES
('http://blogs.packtpub.com/dotnet'),
('http://blogs.packtpub.com/dotnetcore'),
('http://blogs.packtpub.com/signalr')
GO
```

我们已经查看了 `Blog` 表的 SQL 脚本，现在让我们看一下 `Post` 表的脚本，它将引入一个非聚集索引和外键。

#### Post 实体脚本

我们将创建 `Post` 表和一个非聚集索引以获得更好的性能，然后修改它们以添加主键和外键约束，最后向表中插入一些示例数据。完整的脚本可在 GitHub 仓库中获得，地址为 `https://github.com/PacktPublishing/Mastering-Entity-Framework-Core/blob/master/Chapter%202/Final/MasteringEFCore.DatabaseFirst.Final/dbo.Post.sql`。

创建 Post 表并插入数据所需的脚本如下所示：

```sql
// 为简洁起见，代码已移除
CREATE TABLE [dbo].[Post] (
    [Id] INT IDENTITY (1, 1) NOT NULL,
    [BlogId] INT NOT NULL,
    [Content] NVARCHAR (MAX) NULL,
    [PublishedDateTime] DATETIME2 (7) NOT NULL,
    [Title] NVARCHAR (MAX) NOT NULL
);
GO
// 为简洁起见，代码已移除
INSERT INTO [Post] ([BlogId], [Title], [Content], [PublishedDateTime]) VALUES
(1, 'Dotnet 4.7 Released', 'Dotnet 4.7 Released Contents', '20170424'),
(2, '.NET Core 1.1 Released', '.NET Core 1.1 Released Contents', '20170424'),
(2, 'EF Core 1.1 Released', 'EF Core 1.1 Released Contents', '20170424')
GO
```

![](img/17e01f90db603e72c0f0c6a7dce8920c_51_0.png)

插入时间值时，我们应使用 YYYYMMDD 格式，例如 20170424，如果我们习惯使用 DD-MM-YYYY hh:mm:ss xm 格式，则需要执行显式转换，例如 `convert(datetime,'24-04-2017 01:34:09 PM',5)`，否则我们会收到消息：varchar 数据类型到 datetime 数据类型的转换导致值超出范围。

我们需要弄清楚如何使用 LocalDB 执行上述脚本：

1. 从“视图”菜单中打开 SQL Server 对象资源管理器。
2. 展开 SQL Server 和 (localdb)\MSSQLLocalDB。
3. 右键单击“数据库”，然后选择“添加新数据库”。
4. 在“创建数据库”对话框中，将数据库名称提供为 MasteringEFCoreDbFirst，数据库位置提供为你的项目路径，然后点击“确定”。
5. 展开“数据库”，右键单击 MasteringEFCoreDbFirst 数据库，然后选择“新建查询”。
6. 复制 `Blog.sql` 内容并将其粘贴到**新建查询**窗口中，然后点击执行图标或按 Ctrl+Shift+E：

   Blog 脚本的执行如下所示：

   ![](img/17e01f90db603e72c0f0c6a7dce8920c_52_0.png)

7. 复制 `Post.sql` 内容并将其粘贴到**新建查询**窗口中，然后点击执行图标或按 Ctrl+Shift+E：

   Post 脚本的执行如下所示：

   ![](img/17e01f90db603e72c0f0c6a7dce8920c_53_0.png)

   在“新建查询”窗口上执行 Post.sql

我们已经为数据库优先方法准备好了数据库；现在让我们看看如何在现有数据库上利用实体框架。

### 创建新项目

我们已经在第 1 章《快速入门——实体框架核心简介》中详细了解了如何创建新项目。请参考创建项目的相关步骤，并使用以下项目信息：

项目名称：MasteringEFCore.DatabaseFirst

解决方案名称：MasteringEFCore

### 安装实体框架

实体框架包的包含和所涉及的步骤也在第 1 章《快速入门——实体框架核心简介》中进行了广泛讨论。因此，让我们关注逆向工程（数据库优先方法）所需的包。实体框架与 SQL Server 集成所需的基本包如下：

在 PM 控制台中添加以下命令以安装以下包：

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

我们也可以使用 NuGet 包管理器窗口搜索并安装 Microsoft.EntityFrameworkCore.SqlServer 包：

![](img/17e01f90db603e72c0f0c6a7dce8920c_54_0.png)

逆向工程（从数据库自动生成模型）所需的包列在下面。我们可以将以下命令添加到 PM 控制台以安装这些包：

