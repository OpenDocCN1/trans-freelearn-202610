
另一个选项是使用 `ModelAdmin` 子类的 `fieldsets` 属性，将表单布局指定为一系列分组字段。`fieldsets` 中的每个分组由一个标题和一个字典组成，字典中包含一个指向字段名字符串列表的 `fields` 键：

```
fieldsets = (
    ("Linkage", {"fields": ("creator", "book")}),
    ("Review content", {"fields": ("content", "rating")}),
)
```

评价表单应如下所示：

![](img/4840a43efd335abd10b93708225692d6_226_0.png)

图 4.59 – 使用 fieldsets 的评价表单

如果我们想省略某个 fieldset 的标题，可以将其设置为 `None`：

```
fieldsets = (
    (None, {'fields': ('creator', 'book')}),
    ('Review content', {'fields': ('content',
                                   'rating')})
)
```

现在，评价表单应如以下截图所示：

![](img/4840a43efd335abd10b93708225692d6_227_0.png)

图 4.60 – 第一个 fieldset 无标题的评价表单

我们已经了解了变更列表和表单定制，接下来可以通过一个综合活动将所学知识付诸实践。

### 活动 4.03 – 定制 Model admin

在我们的数据模型中，`Contributor` 类用于存储图书贡献者的数据；他们可以是作者、撰稿人或编辑。本活动重点在于修改 `Contributor` 类并添加 `ContributorAdmin` 类，以提高 admin 应用的易用性。目前，`Contributor` 变更列表默认显示单列 `FirstNames`，这是基于*第 2 章“模型和迁移”*中创建的 `__str__` 方法。我们将研究一些其他表示方式。以下是帮助你完成活动的步骤：

1. 编辑 `reviews/models.py`，为 `Contributor` 模型添加额外功能。
2. 为 `Contributor` 添加一个 `initialled_name` 方法，该方法不接收参数（类似于 `Book.isbn13` 方法）。
3. `initialled_name` 方法将返回一个字符串，包含 `Contributor.last_names`，后跟逗号和给定名字的首字母缩写。例如，对于 `first_names` 为 `Jerome David`、`last_names` 为 `Salinger` 的 `Contributor` 对象，`initialled_name` 将返回 `Salinger, JD`。
4. 用调用 `initialled_name()` 的方法替换 `Contributor` 的 `__str__` 方法。

此时，**Contributors** 显示列表将如下所示：

![](img/4840a43efd335abd10b93708225692d6_229_0.png)

图 4.61 – Contributors 显示列表

5. 编辑 `reviews/admin.py` 文件。我们将修改在*列表显示字段*部分中从 `admin.ModelAdmin` 子类化的 `ContributorAdmin`。

6. 修改它，使 Contributors 变更列表上的记录显示为两列可排序字段：Last Names 和 First Names。我们可以从 `ContributorAdmin` 中移除现有的 `list_display` 属性以及 `initialled_name` 函数。

7. 添加一个搜索栏，用于搜索 Last Names 和 First Names。并修改它，使其只匹配 Last Names 的开头。

8. 添加一个基于 Last Names 的过滤器。

完成活动后，你应该会看到类似这样的结果：

![](img/4840a43efd335abd10b93708225692d6_230_0.png)

图 4.62 – 预期输出

可以做出诸如此类的更改，以改进 admin 用户界面的功能。通过在 Contributors 变更列表中将 First Names 和 Last Names 列实现为独立的列，我们让用户可以选择按任一字段排序。通过考虑哪些列在搜索检索和筛选选择中最有用，我们可以提高记录检索的效率。

> **注意**
> 本活动的解决方案可在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions 上找到。

### 摘要

在本章中，我们了解了如何通过 Django 命令行创建超级用户，以及如何使用超级用户访问 admin 应用。然后，在简要浏览了 admin 应用的基本功能之后，我们探讨了如何将模型注册到 admin 应用中，为数据生成 CRUD 界面。

接着，我们学习了如何通过修改站点级功能来改进这个界面。我们通过向 admin 站点注册自定义模型 admin 类，改变了 admin 应用向用户展示模型数据的方式。这使我们能够对模型界面的表示进行细粒度的更改。这些修改包括：通过添加额外的列、过滤器、日期层级和搜索栏来定制变更列表页面。我们还通过分组和排除字段来修改模型 admin 页面的布局。

这只是对 admin 应用功能的浅尝辄止。我们将在第 10 章“高级 Django Admin 与定制”中重新探讨 `AdminSite` 和 `ModelAdmin` 的丰富功能。但首先，我们需要学习 Django 的一些中级功能。在下一章中，我们将学习如何组织并服务静态内容，例如 CSS、JavaScript 和图片，以及如何从 Django 应用中提供这些内容。

## 服务静态文件

一个仅包含纯超文本标记语言（HTML）的 Web 应用是非常有限的。我们可以使用层叠样式表（CSS）和图片来增强网页的外观，并使用 JavaScript 添加交互。我们将所有这些类型的文件称为“静态文件”。它们在开发完成后作为应用的一部分被部署。我们可以将此与动态响应进行比较，动态响应是在发出请求时实时生成的。你编写的所有视图都是通过渲染模板来生成动态响应的。请注意，我们不会将模板视为静态文件，因为它们不是原样发送给客户端的；而是先渲染，然后作为动态响应的一部分发送。

在开发期间，静态文件在开发者的机器上创建，然后必须移动到生产 Web 服务器。如果你必须在短时间内（例如，几小时）上线，那么收集所有静态资源、将它们移动到正确的目录并上传到服务器可能会非常耗时。在使用其他框架或语言开发 Web 应用时，你可能需要手动将所有静态文件放入 Web 服务器托管的特定目录。更改服务静态文件的 URL 可能意味着需要更新代码中的多处值。

Django 可以帮助我们管理静态资源，使这一过程更加容易。它提供了在开发期间使用其开发服务器提供静态文件的工具。当你的应用上线时，它还可以收集所有资源并将它们复制到一个文件夹中，供专门的 Web 服务器托管。这使你可以再开发期间以有意义的方式隔离静态文件，并在部署时自动打包它们。

此功能由 Django 内置的 `staticfiles` 应用提供。它添加了几个用于处理和服务静态文件的有用功能：

- `static` 模板标签会自动构建资源的静态 URL 并将其包含在 HTML 中。
- 一个名为 `static` 的视图在开发环境中提供静态文件。
- 静态文件查找器可用于自定义在文件系统中哪里可以找到资源。
- `collectstatic` 管理命令会查找所有静态文件并将它们移动到一个目录中以便部署。
- `findstatic` 管理命令会显示针对特定请求加载了磁盘上的哪个静态文件。这也可以帮助我们调试某个文件是否未加载。

在本章的练习和活动中，我们将向 Bookr 应用添加静态文件（图片和 CSS）。在开发期间，每个文件都将存储在 Bookr 项目目录内。我们需要为每个文件生成一个 URL，以便模板可以引用它们，并且浏览器可以下载它们。URL 生成后，Django 需要提供这些文件。当我们把 Bookr 应用部署到生产环境时，需要找到所有静态文件并将它们移动到一个可以由生产 Web 服务器提供服务的目录中。如果有静态文件未按预期加载，我们需要某种方法来确定原因。

为了简单起见，让我们以单个静态文件为例：`logo.png`。我们将简要介绍前面提到的每个功能的作用，并在本章中深入解释。我们将介绍静态文件应用的五个部分：

- `static` 模板标签用于将文件名转换为可在模板中使用的 URL 或路径，例如从 `logo.png` 到 `/static/logo.png`。
- `static` 视图接收在 `/static/logo.png` 路径加载静态文件的请求。它读取文件并将其发送到浏览器。
- 静态文件查找器（或简称 `finder`）由 `static` 视图用于在磁盘上定位静态文件。有不同的查找器，但在本例中，其中一个只是将 `/static/logo.png` URL 路径转换为磁盘上的路径 `bookr/static/logo.png`。
- 部署到生产环境时，必须使用 `collectstatic` 管理命令。这将把 `logo.png` 文件从 Bookr 项目目录复制到 Web 服务器目录，例如 `/var/www/bookr/static/logo.png`。
- 如果静态文件无法正常工作（例如，对其请求返回 `404 Not Found` 响应，或提供了错误的文件），那么我们可以使用 `findstatic` 管理命令来尝试确定原因。该命令以文件名作为参数，并输出查看了哪些目录以及在哪里找到了请求的文件。

这些是日常最常用的功能，但我们还将讨论其他功能。

在本章中，你将首先学习静态响应和动态响应之间的区别。然后你将看到 Django 的 `staticfiles` 应用如何帮助管理静态文件。当你继续开发 Bookr 应用时，你将使用图片和 CSS 来增强它。你将看到为项目布局静态文件的不同方式，并了解 Django 如何为生产部署整合这些文件。Django 包含了用于在模板中引用静态文件的工具；你将看到这些工具如何帮助减少部署应用到生产环境时必须做的工作。在此之后，
