# Exploring Django Admin and Management Commands

在 *Chapter 3* 中，我们学习了如何将 DRF 序列化器与视图集成，以及客户端应用程序如何与 Django 视图交互。我们的用户将使用客户端应用程序通过 REST API 执行 CRUD 操作。但在实际应用中，我们需要创建一些额外的接口，让管理员用户执行某些 CRUD 操作。Django 开箱即用地提供了一个管理界面来满足此用例。

在本章中，我们将学习如何使用 Django Admin，它提供了很大的灵活性来配置管理界面，以支持不同的业务用例。我们将学习如何探索这些配置，以及如何使用管理命令更好地与框架交互（例如执行数据库迁移或启动开发服务器）。Django 还提供了扩展这些命令的接口，以便我们可以为自定义用例创建自定义管理命令。我们也将对此进行探讨。

本章将涵盖以下主题：

*   Exploring Django Admin
*   Customizing Django Admin
*   Optimizing Django Admin for production
*   Creating custom management commands

## Technical requirements

在本章中，我们将学习如何使用 Django Admin 面板。Django Admin 与 Django 模型紧密配合，因此您需要对 Django 模型有良好的理解。

本章中使用的代码片段可以在本书的 GitHub 仓库中找到，地址为 [`github.com/PacktPublishing/Django-in-Production/tree/main/Chapter04`](https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter04)。

## Exploring Django Admin

对于任何应用程序，我们都需要某种管理 UI 来执行管理任务。Django Admin 开箱即用地提供了这种强大的功能。但在查看界面之前，像其他任何管理界面一样，我们需要用户登录凭据来访问 Django Admin。

### Creating a superuser in Django

Django 为我们提供了内置的 `createsuperuser` 管理命令来创建新的管理员用户。一旦我们在终端中运行此命令，我们将看到一个引导向导，帮助我们创建 Django 超级用户：

```
python manage.py createsuperuser
```

按照说明并输入提供的数据后，我们将获得一个 Django 超级用户。

Django authentication and authorization

Django 开箱即用地提供身份验证和授权支持。当我们在 Django 中创建超级用户时，我们将使用默认的 Django 身份验证系统。我们将在 *Chapter 5* 中讨论 Django 身份验证和授权的细节。

一旦我们创建了超级用户，我们就可以运行 Django 服务器以登录管理界面。默认情况下，Django Admin 在主 `urls.py` 文件中配置为 `/admin` 路由。现在，让我们学习如何配置 Django Admin 界面以及 Django 为我们与 Django Admin 协作提供的所有功能。

### Understanding the Django Admin interface

当我们在浏览器中访问 `http://127.0.0.1:8000/admin` 时，我们将被重定向到管理员登录页面。UI 界面由 Django 自动生成，开箱即用。一旦我们输入之前创建的超级用户管理员的凭据，我们将能够登录管理界面，如 *Figure 4**.1* 所示：

![Figure 4.1: Default Django Admin interface](img/B18867_04_01.jpg)

Figure 4.1: Default Django Admin interface

目前，我们还没有配置任何额外内容。所以，让我们向 Django Admin 添加一个博客表界面。每个应用都有一个 `admin.py` 文件，帮助我们为给定应用设置 Django Admin。然后，Django Admin 模块帮助我们创建可用于向数据库添加/编辑数据的用户界面。

例如，一旦我们将以下代码添加到 `blog/admin.py`，它就会将 `BlogAdmin` 类注册到 Django Admin UI。这里，我们在代码示例中将 `BlogAdmin` 链接到 `Blog` 模型：

```
from django.contrib import admin
from blog import models
class BlogAdmin(admin.ModelAdmin):
    pass
admin.site.register(models.Blog, BlogAdmin)
```

*Figure 4**.2* 显示了带有新 **BLOG** 部分的 Django Admin UI。随着我们添加新的管理员类并注册它们，它们将开始出现在 UI 中：

![Figure 4.2: The BLOG section in the Django Admin UI](img/B18867_04_02.jpg)

Figure 4.2: The BLOG section in the Django Admin UI

现在，让我们使用更多配置来配置 Django Admin，以改善 Admin 页面的用户体验。首先，让我们看看博客模型的列表页面在 Django Admin 中是什么样子的。*Figure 4**.3* 显示了博客模型的 Django Admin 列表页面：

![Figure 4.3: The Django Admin UI for the blog model](img/B18867_04_03.jpg)

Figure 4.3: The Django Admin UI for the blog model

在 *Figure 4**.3* 中，我们突出了 Django Admin 中负责 UI 的不同类型的配置。在以下代码片段中，我们可以看到映射到相关管理员 UI 的属性。`BlogAdmin` 类中定义的 `search_fields`、`show_full_result_count` 和 `list_filter` 字段负责屏幕上显示的 UI 元素：

```
from django.contrib import admin
from blog import models
class BlogCustomAdmin(admin.ModelAdmin):
    search_fields = ['title']
    show_full_result_count = True
    list_filter = ['title']
    list_display = ['title', 'created_at']
    date_hierarchy = 'created_at'
admin.site.register(models.Blog, BlogCustomAdmin)
```

同样，我们可以通过为 `BlogAdmin` 类定义更多属性来配置 `blog` 管理页面的详细 UI。

Read more

官方 Django 文档展示了我们可以用来自定义 Django Admin 体验的所有开箱即用选项：[`docs.djangoproject.com/en/stable/ref/contrib/admin/`](https://docs.djangoproject.com/en/stable/ref/contrib/admin/)。





到目前为止，我们已经学习了如何在 Django Admin 中显示 Django 模型字段。在下一节中，我们将学习如何使用自定义字段和其他最佳实践来自定义 Django Admin。

# 自定义 Django Admin

在本节中，我们将学习如何自定义 Django Admin 以改善用户体验并解决时不时出现的各种基础但棘手的需求。

## 添加自定义字段

在创建界面时，在 Django Admin 中添加自定义字段是最常见需求之一。通过这样做，我们可以在 Django Admin 中添加 Django 模型中不存在的字段/信息。例如，要在 Django Admin 中创建 `word_count` 字段，我们可以使用以下代码：

```
class BlogCustom2Admin(admin.ModelAdmin):
    list_display = ('title', 'word_count', 'id')
    def word_count(self, obj):
        return obj.content.split()
admin.site.register(models.Blog, BlogCustom2Admin)
```

在这个示例中，我们向 `list_display` 添加了一个名为 `word_count` 的自定义字段，该字段是使用其他字段值计算得出的。

## 使用 filter_horizontal

Django Admin 使我们能够在 Django Admin 中使用 `ManyToManyField`。然而，Django Admin 提供的 `ManyToManyField` 默认界面并不友好。

如 *Figure 4*.4* 所示，当我们使用默认的 admin 界面时，很难识别所选字段：

![Figure 4.4: Default admin interface for ManyToManyField](img/B18867_04_04.jpg) 

Figure 4.4: Default admin interface for ManyToManyField

为了改善 admin 用户体验，我们可以将 `tags` 字段传递给 `BlogAdmin` 类中的 `filter_horizontal` 属性。这会将 `ManyToManyField` 的 admin 界面转换为更好的界面：

```
class BlogCustom3Admin(admin.ModelAdmin):
    filter_horizontal = ['tags']
admin.site.register(models.Blog, BlogCustom3Admin)
```

使用 `filter_horizontal` 属性，我们可以获得更好的界面，如 *Figure 4*.5* 所示。我们可以识别已选择的字段和可供选择的字段：

![Figure 4.5: The admin interface when we use the filter_horizontal attribute](img/B18867_04_05.jpg) 

Figure 4.5: The admin interface when we use the filter_horizontal attribute

你也可以使用 `filter_vertical` 来改善界面。

## 使用 get_queryset

Django Admin 界面中显示的数据是通过 QuerySet 获取的。在 *Chapter 2* 和 *Chapter 3* 中，我们看到了如何通过使用适当的查询来获得性能提升。在使用 Django Admin 时，我们会遇到在 *Chapter 3* 中讨论的相同的 N+1 select 查询问题。为了解决这个问题，我们可以使用 `select_related` 或 `prefetch_related` 查询来覆盖默认的 Django Admin QuerySet。

在下面的示例中，我们尝试通过访问 `author.user` 表数据来获取用户的全名：

```
class BlogCustom4Admin(admin.ModelAdmin):
    list_display = ['title', 'created_at', 'author_full_name']
    def author_full_name(self, obj):
        return f'{obj.author.user.first_name} {obj.author.user.last_name}'
    def get_queryset(self, request):
        default_qs = super().get_queryset(request)
        improved_qs = default_qs.select_related('author', 'author__user')
        return improved_qs
admin.site.register(models.Blog, BlogCustom4Admin)
```

由于 `author_full_name` 字段已添加到 `list_display`，Django Admin 将为每一行执行两次查询。例如，如果我们有 10 篇博客文章，它将执行 20 次 DB 查询来获取作者姓名信息。当我们有数百条记录时，这会带来性能损失，因此我们覆盖默认的 `get_queryset` 方法，通过使用 `select_related` 查询来提高性能。

同样，我们可以使用不同的查询来覆盖默认的 `get_queryset` 方法以获得性能提升。强烈建议在应用程序增长时监控 Django Admin 的数据访问模式；这些小的性能瓶颈很容易解决。

## 使用第三方包和主题

我们可以使用多个第三方包来增强 Django Admin 界面。虽然是否应该使用第三方包是一个有争议的话题，但我通常更喜欢在 Django Admin 用于基本 CRUD 操作以进行管理目的时使用默认界面。

以下 GitHub 仓库列出了所有出色的 Django Admin 资源：[`github.com/originalankur/awesome-django-admin`](https://github.com/originalankur/awesome-django-admin)。

## 使用 Django Admin 日志

在任何应用程序中，记录用户执行的管理员操作都非常重要。Django 非常重视安全，并开箱即用地提供了管理员操作日志界面。

默认情况下，Django 仅在 admin 主页上显示最近记录的更改，如 *Figure 4*.6* 所示：

![Figure 4.6: Showing the Django Admin action logs](img/B18867_04_06.jpg) 

Figure 4.6: Showing the Django Admin action logs

一旦我们添加以下代码片段以启用 `LogEntryAdmin`，我们将允许用户执行任何只读操作并阻止任何写操作：

```
from django.contrib.admin.models import LogEntry
@admin.register(LogEntry)
class LogEntryAdmin(admin.ModelAdmin):
    def has_change_permission(self, request, obj=None):
        return False
    def has_delete_permission(self, request, obj=None):
        return False
    def has_add_permission(self, request):
        return False
```

我们应该确保 `LogEntryAdmin` 的访问受到限制并且是只读的。请注意，我们已覆盖 `has_<action>_permission` 方法，以便用户只能读取操作而不能操作任何过去的操作。在内部，Django 将所有这些日志信息存储在 `django_admin_log` 表中。

现在，让我们开始研究如何在生产环境中使用 Django Admin，并学习所有最佳实践和技巧，帮助我们更好地使用 Django Admin。

# 为生产环境优化 Django Admin

在使用生产环境中的 Django Admin 时，有一些提示和技巧你应该遵循，以充分利用 Django。我亲身经历了很多挑战，因为开发人员会让 Django Admin 承担越来越多的管理任务。因此，让我们来看一些良好的实践和优化措施，帮助我们充分发挥 Django Admin 的潜力。

## 重命名 admin URL

默认情况下，Django Admin 配置为在 `/admin` 路径下访问。然而，Django Admin 可以访问你网站的所有数据；这就是为什么将路径从 `/admin` 移动到一些难以猜测的路径，例如 `/blog-cms` 很重要。

不同的包可以让我们提高安全性。例如，`django-admin-honeypot` ([`github.com/dmpayton/django-admin-honeypot`](https://github.com/dmpayton/django-admin-honeypot)) 是一个创建虚假 admin 页面并诱骗攻击者访问虚拟 admin 界面的包。

## 为管理员用户使用双因素认证 (2FA)

Django Admin 可由内部用户使用来执行需要额外权限的管理任务；由于这些用户拥有额外权限，保护任何安全漏洞应该是最高优先级。为 Django Admin 用户添加 2FA 在安全方面非常重要。我们不会在这里深入探讨使用 Django 实现 2FA，因为这些实现因人而异，但其中一个推荐的第三方包可以帮助你在 Django 中设置 2FA 的是 `django-otp`，你可以在这里了解更多信息：[`github.com/django-otp/django-otp`](https://github.com/django-otp/django-otp)。

## 使用自定义 Admin 分页器





Django Admin 界面已实现了分页。随着表格数据的增长，默认分页的性能会下降。这主要是由于对大型表执行 `count` 查询时，由于表中存在大量数据，耗时更长；查询会扫描所有行以确定条目的最终计数。我们应该用自定义分页器覆盖默认分页器，该分页器重写了 `count` 方法。例如，我们创建了 `CustomPaginator`，它重写了 `count` 行为：

```
from django.core import paginator
from django.utils.functional import cached_property
# Idea referred from
# https://hakibenita.com/optimizing-the-django-admin-paginator
class CustomPaginator(paginator.Paginator):
    @cached_property
    def count(self):
        return 9999999
class BlogCustom5Admin(admin.ModelAdmin):
    paginator = CustomPaginator
admin.site.register(models.Blog, BlogCustom5Admin)
```

现在，Django Admin 不会再对表执行数据库调用以获取总行数，并且默认返回一个巨大的数字（`9999999`），如代码中所定义。我们应该仅将此方法用于大型表，因为我们返回的是一个常量计数，可能会让最终用户感到困惑。

## 禁用 ForeignKey 下拉选项

Django 模型中的 `ForeignKey` 字段会在 Django 模型管理界面中自动添加下拉选项。如果我们有一个大型表，这会造成性能瓶颈，因为 Django 会尝试将所有外键对象加载到内存中并在下拉选项中显示。Django 理解这样做的性能不足之处，因此它为我们提供了 `raw_id_fields` 接口，该接口告诉 Django 用自由文本字段替换下拉选项：

```
class BlogCustom6Admin(admin.ModelAdmin):
    raw_id_fields = ('author',)
admin.site.register(models.Blog, BlogCustom6Admin)
```

## 使用 list_select_related

当我们使用 `list_display` 访问关联字段时，会遇到 N+1 查询问题。之前我们看到如何重写默认的 `get_queryset` 方法来进行优化。但 Django Admin 提供了 `list_select_related` 属性来告诉 Django 在后台使用 `select_related` 查询。如 *第 3 章* 中所述，使用 `select_related` 获取信息可以解决 N+1 查询问题。

例如，在这里，我们将 `author` 和 `author__name` 作为列表值传递给 `list_select_related`，它会自动执行 select-related 查询：

```
class BlogCustom7Admin(admin.ModelAdmin):
    list_display = ['title', 'created_at', 'author_full_name']
    list_select_related = ['author', 'author__name']
    def author_full_name(self, obj):
        return obj.author.name
admin.site.register(models.Blog, BlogCustom7Admin)
```

请注意，如果我们尝试执行反向关系，`list_select_related` 就不起作用。因此，我们需要重写 `get_queryset` 方法，正如我们之前所学的。

## 为性能重写 get_queryset

我们应该使用 `get_queryset` 来优化 Django 使用的查询。当我们有反向外键关系查找时，这尤其有帮助。我们已在 *使用 `get_queryset`* 部分讨论了如何重写 `get_queryset` 方法。

## 添加 django-json-widget

由于 Postgres 和其他数据库支持 JSON 字段，你很可能会在 Django 中使用 JSON 字段。其中许多字段将通过 Django Admin 手动访问。Django Admin 为 JSON 字段提供的默认 UI 是一个 `textarea` UI，但这并不友好。解决此问题的最有用的第三方包之一是 `django-json-widget`（`https://github.com/jmrivas86/django-json-widget`）。该包有助于为 JSON 字段提供更好的界面。

*图 4.7* 显示了管理面板中常规的 Django JSON 编辑器：

![Figure 4.7: Showing the regular Django JSON editor](img/B18867_04_07.jpg)

图 4.7：显示常规的 Django JSON 编辑器

现在，如果我们使用 `django-json-widget`，我们可以使用更好的 JSON 编辑器；其结构如 *图 4.8* 所示：

![Figure 4.8: Showing the django-json-widget editor](img/B18867_04_08.jpg)

图 4.8：显示 django-json-widget 编辑器

## 使用自定义操作

通常，管理员用户会使用管理界面执行一组重复的操作。这意味着他们必须在多个页面之间手动跳转（例如，如果我们要让博客上线，我们必须打开每个页面并为每个博客更新状态标志）。这类操作可以很容易地使用自定义操作来执行。

例如，让我们创建一个自定义操作来打印所选博客的标题：

```
class BlogCustom8Admin(admin.ModelAdmin):
    actions = ('print_blogs_titles',)
    @admin.action(description='Prints title')
    def print_blogs_titles(self, request, queryset):
        for data in queryset.all():
            print(data.title)
admin.site.register(models.Blog, BlogCustom8Admin)
```

这段代码片段在 Django Admin 中为我们提供了一个额外的下拉选项，称为 **Prints title**，如 *图 4.9* 所示。新的操作选项 **Prints title** 将在运行 Django 服务器的终端中打印所有博客标题：

![Figure 4.9: Showing the newly added Action option called Prints title in Django Admin page.](img/B18867_04_09.jpg)

图 4.9：显示 Django Admin 页面中新增的操作选项 Prints title。

## 为 Django Admin 使用权限

Django Admin 界面帮助我们通过 UI 无缝执行 CRUD 操作。这也意味着如果有人可以访问 Django Admin 面板，他们将可以不受限制地访问用户数据。Django 原生提供组和权限，以帮助我们在 Django Admin 中实现不同权限级别。我们将在 *第 5 章* 中学习如何高效使用 Django 权限。

### 避免为功能使用 Django Admin

随着项目的成熟，功能需求也会不断演变。Django Admin 最常用于为管理相关任务的 CRUD API 创建 UI。然而，它绝不应该被用作面向客户的功能。如果你发现自己在 Django Admin 之上构建功能，那么这就是退一步并在其他框架或平台中构建 UI 的正确时机。

如果管理不当，Django Admin 很容易成为权限噩梦。始终建议在产品开发的早期阶段使用 Django Admin。随着产品/公司的成熟，你应该开始考虑将用户从 Django Admin 中迁移出去。

至此，我们已经学习了如何在生产环境中使用 Django Admin，以及如何配置 Django Admin 以获得更好、更可扩展的可用性。现在，让我们学习如何在 Django 中创建自定义管理命令，这被广泛使用。

# 创建自定义管理命令

Django 管理命令帮助我们无缝地与 Django 框架交互。许多繁琐的任务，如迁移、获取构建文件等，如果使用 Django 管理命令，就会变得更加顺畅。Django 原生提供了许多管理命令，可以在文档中找到：`https://docs.djangoproject.com/en/stable/ref/django-admin/`。





在本节中，我们将学习如何在 Django 中创建自定义管理命令。在 Django 中，每个应用程序都可以使用 `manage.py` 注册其自定义操作。要创建新的管理命令，我们需要在 Django 应用内部创建一个简单的目录结构。在给定的 Django 应用中，我们需要创建 `management/commands` 目录结构，如 *图 4*.10* 所示。`commands` 文件夹中创建的任何文件都会被注册为 Django 管理命令，除非文件名以下划线 (`_`) 开头。

在 *图 4*.10* 所示的示例中，我们有 `publishedblogs` 和 `totalblogs` 作为两个管理命令，而 `_private` 则不会被视为管理命令：

![Figure 4.10: The folder structure for custom management commands](img/B18867_04_10.jpg) 

图 4.10：自定义管理命令的文件夹结构

请注意，只有当给定应用已添加到 `INSTALLED_APPS` 设置中时，Django 管理命令才会被注册。一旦我们创建了管理命令文件，我们需要在自定义管理命令文件中定义一个名为 `Command` 的类，该类继承自 `BaseCommand`。例如，我们的 `total_blogs.py` 文件将包含以下代码：

```
from django.core.management.base import BaseCommand
from blog.models import Blog
class Command(BaseCommand):
    help = 'Returns total number of blogs'
    def handle(self, *args, **options):
        total_blogs = Blog.objects.count()
        self.stdout.write(self.style.SUCCESS(f'Total blogs: "{total_blogs}"'))
```

`handle` 方法计算博客表中博客的总数，并使用 `self.stdout.write()` 将其打印到终端。我们可以通过使用 `python manage.py total_blogs` 以相同的方式执行我们的 `total_blogs` 自定义命令。

有时，我们需要向自定义命令传递参数。我们可以通过向 `Command` 类添加一个名为 `add_arguments` 的新方法来实现。例如，在这里，我们添加了 `add_arguments` 方法，该方法配置了可以传递给我们的 `custom_command_info` 命令的两个参数。第一个参数 `author_ids` 是一个必需的位置参数，而 `custom` 是一个可选参数：

```
class Command(BaseCommand):
    help = 'Returns total number of blogs'
    def add_arguments(self, parser):
        # Positional arguments
        parser.add_argument('custom_inputs', nargs='+', type=int)
        # Named (optional) arguments
        parser.add_argument(
            '--custom',
            type=int,
            help='custom optional param',
        )
    def handle(self, *args, **options):
        print(f'Custom inputs - {options["custom_inputs"]}')
        print(f'Custom - {options["custom"]}')
        total_blogs = Blog.objects.count()
        self.stdout.write(self.style.SUCCESS(f'Total blogs: "{total_blogs}"'))
```

如果我们运行 `custom_command_info` 命令，那么我们将在终端中看到以下输出：

```
> python manage.py custom_command_info 12 45 --custom 99
Custom inputs – [12, 45]
Custom – 99
Total blogs: "3"
```

Django 管理命令完全可定制，我们甚至可以用自己的逻辑覆盖默认的管理命令。有关如何自定义 Django 管理命令的更详细信息，请查看官方文档：https://docs.djangoproject.com/en/stable/howto/custom-management-commands/。

# Summary

在本章中，我们学习了如何在项目中使用 Django Admin。Django Admin 既可定制又可扩展，如果我们使用适当的配置，就能充分发挥其功能。我们还探讨了何时应避免使用 Django Admin 以及开发人员面临的性能问题。然后，我们学习了 Django 的另一个强大功能，即创建自定义管理命令。

我们简要学习了如何在 Django 中创建超级用户，但我们没有深入探讨身份验证和授权的工作原理。在下一章中，我们将探讨如何更好地使用 Django 身份验证系统以及如何扩展它以支持不同类型的身份验证，例如社交身份验证、基于令牌的身份验证等。

# 5

# Mastering Django Authentication and Authorization

在 *Chapter 4* 中，我们学习了如何使用 Django Admin 为基本 CRUD 操作自动生成 `user interface` (`UI`)。在使用 Django Admin 的过程中，我们使用了默认的 Django 身份验证和授权系统。在本章中，我们将深入 Django 身份验证系统，并学习如何使用 Django 组和权限来实现资源的授权。Django 默认提供基于 cookie 的会话身份验证，这对于基于浏览器的应用程序非常完美，但在现代 Web 应用中，更倾向于使用基于令牌的身份验证。`Django REST framework` (`DRF`) 开箱即用地提供基于令牌的身份验证，我们将学习如何将 DRF 基于令牌的授权集成到我们的项目中以及社交登录。

在本章中，我们将涵盖以下主要主题：

* 学习 Django 身份验证的基础知识
* 自定义 `User` 模型
* 使用 `OneToOneField` 与 `User` 模型的关系
* 使用 Django 权限和组
* 使用 DRF 基于令牌的身份验证
* 了解第三方基于令牌的身份验证包
* 将社交登录集成到 Django 和 DRF

# Technical requirements

您应该熟悉身份验证的基本概念以及为什么 Web 应用程序需要身份验证。我们将介绍如何在 Django 中实现 `role-based access control` (`RBAC`)，因此您需要对 RBAC 的概念有基本的了解。您还应该熟悉 Google 和其他社交登录提供商用于为不同应用提供身份验证服务的 `OAuth 2.0` 的核心概念。

以下是本章所有代码和说明的 GitHub 仓库：https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter05

# Learning the basics of Django authentication

当今世界，任何用户在使用应用程序时最关心的问题是，*我的数据在该应用程序中安全吗？* 这也是开发人员最应该牢记的事情——*始终开发安全的应用程序*。那么，*安全* 究竟意味着什么？安全可以分为多个层面，例如身份验证、授权、服务器安全（即应用程序运行的服务器是安全的）等等。在本章中，我们将重点关注安全的最基本部分，即 `application authentication`；即通过 `login`。Django 附带了一个内置的身份验证框架。

Django 的开箱即用方法提供了一个开箱即用的身份验证系统，它为我们提供了一个基本的 `user model`，并集成了基于会话（cookie）的身份验证。当我们创建一个新的 Django 项目时，默认情况下，Django 会在 `settings.py` 的 `INSTALLED_APPS` 部分包含 `django.contrib.auth` 和 `django.contrib.sessions`，这为我们提供了默认的 `User` 模型和基于会话的身份验证。在 *Chapter 4* 中，我们使用了管理面板，它使用相同的基于会话的身份验证来提供用户登录工作流程。

Read more





基于会话的身份验证本身就是一个庞大的主题，超出了本书的范围。如果你不熟悉基于会话的身份验证概念，请阅读官方文档 [`docs.djangoproject.com/en/stable/topics/http/sessions/`](https://docs.djangoproject.com/en/stable/topics/http/sessions/)。

Django 默认提供了一个标准的 `User` 模型，其中包含以下字段：

*   `username`：必填字段，最大长度为 150 个字符。用于登录目的。请记住——这不是电子邮件地址，因此可以使用此字段作为登录的唯一标识符。我见过组织将此字段用作电子邮件、电话号码或自定义用户名，因此请随意将此字段用作任何字段。
*   `first_name`：可选字段，最大长度为 150 个字符。
*   `last_name`：可选字段，最大长度为 150 个字符。
*   `email`：可选字段，可用于捕获用户的电子邮件地址。
*   `password`：必填字段，保存所设置密码的哈希值。Django 不会在数据库中存储原始密码。
*   `is_staff`：布尔字段，用于检查用户是否可以访问管理站点。
*   `is_active`：布尔字段，用于识别用户账户是否已启用。如果将 `is_active` 字段设置为 `False`，则 Django 提供的默认 `ModelBackend` 身份验证将不允许用户登录。*请注意，如果你正在创建任何自定义登录系统，则除非你已实现自定义逻辑来检查该标志，否则它将不会被尊重。*
*   `is_superuser`：布尔字段，应谨慎使用。如果为任何用户设置此标志，则他们可以完全访问所有 Django 管理部分。
*   `last_login`：日期时间字段，捕获用户上次登录的时间。*请注意，此字段仅在使用 `ModelBackend` 的默认登录机制时更新。*
*   `date_joined`：日期时间字段，捕获用户账户创建时的日期时间。
*   `groups`：与 `Group` 表的多对多关系。我们将在 *Using Django permissions and groups* 部分讨论 *groups* 概念。当我们使用 Django 授权为用户提供访问控制时，这特别有帮助。Django 使用此字段在后端提供 RBAC 支持。
*   `user_permissions`：与 `Permission` 表的多对多关系。Django 将权限与组结合使用，以在管理门户或 API 级别权限上提供 RBAC，使用 `has_perm` 方法。我们将在 *Using Django permissions and groups* 部分讨论 *permissions* 概念。

上述所有字段都是标准的，对于任何基础项目都足够了。许多组织或项目可能有自定义需求，需要与给定用户关联的额外字段，这时我们可以自定义 `User` 模型。在以下部分，我们将学习在 Django 中自定义 `User` 模型的不同方法。

# 自定义 User 模型

主要可以通过两种方法来自定义 `User` 模型。两者都有各自的优势，让我们一个一个地学习：

*   **通过一对一关系扩展 User 模型**：每当需要向用户添加额外字段时，这是一种最简单和最好的解决方案。在这种方法中，我们将创建一个新模型，例如 `UserProfile`，然后与 `User` 模型创建一对一关系。任何新的自定义字段都将添加到 `UserProfile` 模型中。我建议大多数情况下使用这种方法，因为这是最简单、最干净的解决方案之一，不需要开发人员付出太多努力，并且满足我们的需求。
*   **使用 AbstractUser 和 AbstractBaseUser 模型扩展**：这是扩展 `User` 模型的另一种方法。在这种方法中，我们必须创建我们自己的自定义 `User` 模型并继承自 `AbstractUser` 类或 `AbstractBaseUser` 类。这些模型之间的关键区别是：
    *   `AbstractUser`：仅当你想更新 `User` 表中的用户名字段时才使用 `AbstractUser` 模型。
    *   `AbstractBaseUser`：如果你想从头开始在 `User` 表中创建所有字段，请使用 `AbstractBaseUser` 模型。

建议仅在你对 Django 有充分理解并熟悉登录和身份验证工作流程时才使用自定义模型方法。

## 使用 AbstractUser 或 AbstractBaseUser 创建自定义 User 模型

现在，让我们学习如何在 Django 项目中使用 `AbstractUser` 或 `AbstractBaseUser` 模型创建自定义 `User` 模型：

1. 创建一个新的 Django 应用，`custom_user`。
2. 然后，定义一个 `CustomUser` 模型，具有适当的字段，例如 `phone_no` 和 `city`，继承 `AbstractUser` 类。
3. 接下来，将 `username` 字段设置为 `None`。我们还需要从 `BaseUserManager` 定义一个自定义模型管理器：

```
from django.contrib.auth.base_user import BaseUserManager
from django.contrib.auth.models import AbstractUser
from django.db import models
class CustomUserManager(BaseUserManager):
    """
    Used for updating default create user behaviour
    """
    def create_user(self, phone_no, password, **kwargs):
        # implement create user logic
        user = self.model(phone_no=phone_no, **kwargs)
        user.set_password(password)
        user.save()
        return user
    def create_superuser(self, phone_no, password, **kwargs):
        # creates superuser.
        self.create_user(phone_no, password)
class CustomUser(AbstractUser):
    username = None
    phone_no = models.CharField(unique=True, max_length=20)
    city = models.CharField(max_length=40)
    USERNAME_FIELD = "phone_no"
    objects = CustomUserManager()
```

前面的代码片段定义在 `custom_user/models.py` 文件中。在代码片段中，我们创建了一个 `CustomUser` 模型和一个 `CustomUserManager` 模型管理器，并将 `username` 逻辑更新为 `phone_no`。这意味着，从现在开始，我们可以利用 `phone_no` 字段以及密码登录到我们的 Django 应用程序。

4. 一旦我们创建了模型，我们需要告诉 Django 为任何身份验证目的使用新模型。我们需要进入 `settings.py` 文件，然后定义一个新变量：

```
AUTH_USER_MODEL = "custom_user.CustomUser"
```

请注意，`"custom_user"` 是我们自定义应用的名称，其中定义了 `CustomUser` 模型。

5. 现在，运行 Django 迁移命令来创建我们的新 `User` 模型。
6. 我们还需要更新 Django Admin 表单，以便我们的 Django Admin 工具正常工作。我们不会深入表单的细节，因为这超出了本书的范围。

### 阅读更多

Django 的官方文档提供了明确的示例；请在此处查看 [`docs.djangoproject.com/en/stable/topics/auth/customizing/#a-full-example`](https://docs.djangoproject.com/en/stable/topics/auth/customizing/#a-full-example)。

我们已经学习了 Django 如何开箱即用地提供 `User` 模型，以及如何自定义 `User` 模型以适应我们的用例。

尽管我们已经学习了如何在 Django 中使用自定义 `User` 模型，但建议创建一个单独的配置文件模型来捕获所有额外的用户信息，并*使用一对一关系*与默认的 `User` 模型关联，以便 Django 可以处理所有身份验证和其他安全功能。使用这种方法，开发人员可以专注于产品功能开发，而无需过多参与身份验证的框架级更改。





# 使用 OneToOneField 关系与 User 模型

在 Django 中存储自定义用户信息最简单的方法之一是创建新模型并与默认 `User` 模型创建新的 `OneToOneField` 关系。让我们通过一个简单示例来实现这种方法。

我们需要在平台上为每个注册用户存储电话号码和城市，但 Django 默认的 `User` 模型中没有这些字段。我们应该创建一个新模型来为每个用户存储此信息。

首先，创建一个新的自定义 Django 应用 `custom_user`。然后，在 `custom_user/models.py` 文件中添加新的 `UserProfile` 模型：

```
from django.db import models
class UserProfile(models.Model):
    user = models.OneToOneField('auth.User', related_name='user_profile', on_delete=models.CASCADE)
    phone_no = models.CharField(unique=True, max_length=20)
    city = models.CharField(max_length=40)
    def __str__(self):
        return self.user.username
```

在这里，我们创建了一个 `user` 字段，它与 Django 默认的 `User` 模型具有一对一关系。每个用户的电话号码和城市信息存储在 `phone_no` 和 `city` 字段中。

使用这种创建 `UserProfile` 等自定义模型的方法的最大优势在于，我们可以拥有任何不会与 Django 默认 `User` 模型行为冲突的字段和逻辑。

我们看到 Django `User` 模型与 *组和权限* 具有关系。在下一节中，我们将深入探讨 `Group` 和 `Permission` 表的细节，这些表可以帮助我们为终端用户提供 RBAC 支持。

# 使用 Django 权限和组

RBAC 是一种基于分配给单个用户的角色来限制访问的方法。Django 权限和组是我职业生涯中遇到的最深思熟虑、最详细的 RBAC 系统之一。我总是选择 Django 来完成任何紧迫期限的项目的主要原因之一，主要是因为它开箱即用的身份验证和授权系统。在本节中，我们将对如何在项目中使用 Django 组和权限进行高层次概述。

## 在 Django Admin 中使用权限和组

在 *第 4 章* 中，在探索 Django Admin 时，我们使用 Django `superuser` 在管理面板中导航；因此不需要权限。但随着项目进入生产环境，我们希望根据其用例为每个用户提供受限访问。例如，支持代理只需要对所有支付模型具有 *只读* 访问权限，并且不应访问任何其他数据库。这类 RBAC 可以轻松通过 Django Admin 实现。

当我们以超级用户身份登录 Django 管理面板时，我们将能够看到身份验证和授权 UI，如 *图 5**.1* 所示。`Groups` 和 `Users` 部分帮助我们为 Django 应用程序实现 RBAC：

![Figure 5.1: Django admin panel showing AUTHENTICATION and AUTHORIZATION](img/B18867_05_01.jpg) 

图 5.1：显示 AUTHENTICATION 和 AUTHORIZATION 的 Django 管理面板

默认情况下，Django 为每个模型对象创建权限对象。这些权限对象在 Django Admin 中用于检查使用 Django Admin 的特定用户是否有授权执行请求的模型操作。如果我们在 Django Admin UI 中打开 auth `User` 对象，我们将能够看到附加到特定用户的所有组和用户权限。例如，如 *图 5**.2* 所示，我们可以看到可附加到用户的全部用户权限。Django 为项目中所有模型创建权限对象，具有 `add`、`change`、`delete` 和 `view` 等不同操作类型。我们可以选择特定模型，然后将所需权限附加到用户对象：

![Figure 5.2: User object view in Django Admin showing all the groups and user permissions attached to the given user](img/B18867_05_02.jpg) 

图 5.2：Django Admin 中用户对象视图，显示附加到给定用户的所有组和用户权限

尽管我们始终可以将单个权限附加到用户，但最好始终使用组来为任何用户授予权限，以便所有权限都可以通过组集中管理。Django `permissions` 帮助我们创建模型操作的精细控制，而 Django `groups` 是权限的集合，有助于我们创建角色。例如，支持代理可能只需要访问查看 author 模型和 blog 模型，因此我们将创建一个名为 `Support Agent` 的 Django 组并添加 author 和 blog 模型的只读权限，如 *图 5**.3* 所示：

![Figure 5.3: Creating a new Django Support Agent group](img/B18867_05_03.jpg) 

图 5.3：创建新的 Django Support Agent 组

一旦我们创建了 Django `Support Agent` 组，我们将其附加到用户，如 *图 5**.4* 所示。这将允许用户通过 Django Admin 以只读方式访问 `author` 和 `blog` 表：

![Figure 5.4: Attaching the Support Agent group to a Django user](img/B18867_05_04.jpg) 

图 5.4：将 Support Agent 组附加到 Django 用户

使用 Django 组有助于我们从中央位置控制 Django Admin 界面的权限，每个用户类型各不相同。例如，如果我们想删除所有支持代理查看作者信息的能力，我们将转到 `Support Agent` 组并删除附加的作者查看权限；这将自动从所有通过 Django `Support Agent` 组获得访问权限的支持代理中移除对 author 模型的访问。

Django 开箱即用提供基本 CRUD 操作权限对象，但我们的业务用例可能需要自定义权限；例如，只有某些用户可以更新博客标题，而其他用户只能编辑博客内容。对于此类用例，Django 允许我们创建自定义权限。

## 创建自定义权限

Django 权限在模型级别。要创建自定义权限，我们必须使用 permissions 模型 `Meta` 属性。例如，我们需要创建权限以允许某些用户仅更新博客标题，而某些用户更新博客内容：

```
class Blog(models.Model):
  ...
  class Meta:
    permissions = [
      ("update_title", "Can update the title of the blog"),
      ("update_content", "Can update the content of blog"),
    ]
```

一旦我们在模型中定义了权限，我们就需要运行 Django 迁移来创建权限对象。

仅仅创建自定义权限并不意味着我们的应用程序将实现所有自定义权限。我们需要编写自定义逻辑来使用 Django 权限强制执行授权。

让我们创建一个简单的 Django 视图，该视图将使用自定义权限并检查结果：

```
def update_blog_title(request):
    blog_id = request.GET.get('id')
    blog = Blog.objects.get(id=blog_id)
    if request.user.has_perm("blog.update_title"):
        return HttpResponse('User has permission to update title')
    return HttpResponse('User does not have permission to update title')
```

现在，如果我们尝试在浏览器上访问该视图，我们将收到 **User does not have permission to update title** 消息。但如果我们将权限添加到用户然后尝试访问页面，则会收到 **User has permission to update title** 响应。

请注意

如果您以超级用户身份登录 Django Admin 界面，那么您将始终看到 **User has permission to update title**。这是因为在 Django 中，超级用户隐式拥有所有权限。然而，并非每个员工用户都具有全站权限，因此应创建一个普通员工用户并分配自定义权限来测试代码。





Django 自定义权限使我们能够利用现有的 Django 权限框架，为我们的 Django 应用提供自定义的 RBAC 支持。

## 使用 Django 权限和组为 API 服务

Django 组和权限可以通过编写自定义代码用于 API。DRF 还支持自定义 `Permission` 类，可以在内部使用 Django 权限和组：

```
def check_permission(user, group_name):
    return user.groups.filter(name=group_name).exists()
@api_view(['POST'])
def blog_view(request)
    if not check_permission(request.user, 'can_view_blog')
        return Response(status=403)
    # perform operation
```

在前面的代码示例中，我们正在检查发出请求的 user 是否有资格评论。如果 `can_view_blog` 组附加到 user，则他们有资格评论；否则，他们没有资格评论，我们将返回 `403` 错误。

类似地，我们可以在代码中编写自定义业务逻辑来支持 DRF API 的授权。我们也可以编写自定义 DRF 权限类来执行此类操作。

Read more

更多详情，请查阅官方文档了解如何在 DRF 中创建自定义权限类：[`www.django-rest-framework.org/api-guide/permissions/#custom-permissions`](https://www.django-rest-framework.org/api-guide/permissions/#custom-permissions%20)

## 使用权限的注意事项

尽管 Django 组和权限是我遇到过的最强大的 RBAC 系统之一，但我也见过很多项目因过度使用 Django 权限框架而变得难以维护。在本节中，我们将尝试学习所有良好的实践，开发者和团队可以采用这些实践来正确地利用 Django 组和权限：

*   **避免直接将权限附加到 auth user 对象**：当我们开始使用 Django 权限时，直接将权限附加到 django auth user 可能会很诱人，但这种诱惑很快就会导致噩梦。随着我们使用更多 Django 权限，跟踪哪个 user 可以访问 Django 应用的哪个部分变得很棘手。为了解决这个问题，始终使用 Django 组将权限附加到用户。
*   **避免创建超级用户**：我们应避免为项目创建超级用户，因为他们可以通过 Django Admin 无限制地执行任何操作。
*   **避免使用过多的 Django 自定义权限**：Django 权限附加到模型，有时开发者会不必要地将自定义 Django 权限附加到模型，而他们并不需要任何模型关系。尝试为此目的使用 Django 组。

在下面的代码片段中，我们正在检查 user 是否附加到特定组以进行授权：

```
if user.groups.filter(name='custom_group').exists():
    # perform operation
```

`custom_group` 可能没有附加任何模型权限，但基本的 Django 组可用于检查 user 是否属于特定组以进行操作。

到目前为止，我们了解了 Django 权限框架以及如何使用它在 Django 应用中创建 RBAC 系统。现在，我们将研究 DRF 如何支持基于令牌的身份验证，以及如何将其集成到我们的 Django 项目中。

# 使用 DRF 基于令牌的身份验证

现代 Web 应用不再仅限于浏览器。事实上，*超过 55%* 的互联网流量来自移动设备（根据这份 *Cloudflare* 报告：[`radar.cloudflare.com/traffic?range=28d`](https://radar.cloudflare.com/traffic?range=28d)）。Django 最初是在 18 多年前构建的，当时浏览器更流行，但在当今世界，移动设备和应用的数量超过了浏览器，这需要不同类型的身份验证。DRF 开箱即支持多种身份验证类型：

*   基本身份验证
*   基于会话的身份验证
*   基于令牌的身份验证
*   远程用户身份验证

基于令牌的身份验证是当今世界最流行、最广泛使用的身份验证形式。在本节中，我们将学习如何在 Django 项目中启用和使用基于令牌的身份验证，使用基于令牌的身份验证的优势，以及另一种自定义的基于令牌的身份验证。

## 将基于令牌的身份验证集成到 DRF

DRF 基于令牌的身份验证使用简单的基于令牌的 HTTP 身份验证方案。DRF 创建一个新模型，为给定 user 保存已颁发的令牌，并与 `User` 表有一对一的关系。让我们逐步查看 DRF 如何实现基于令牌的身份验证工作流程：

1.  user 发出包含用户名和密码的登录请求。
2.  Django 从 `Auth User` 表中验证用户名和密码。
3.  一旦 user 凭据得到验证，DRF 为给定 user 创建一个新令牌，然后将令牌作为响应发送给最终 user。

之后客户端发出的所有请求现在都将已颁发的令牌附加到 HTTP 头部。
4.  当 Django 收到带有附加到请求的 auth 令牌的请求时，Django 然后将令牌与数据库中保存的令牌进行验证，并将 `Auth User` 对象附加到 `request.user` 属性。这会将 user 详细信息填充到 `request` 对象中，令牌可用于识别哪个 user 发出了请求。如果令牌不匹配，则 DRF 将向客户端发送 `401` 错误。

在本节中，我们在高层次上了解了 DRF 基于令牌的身份验证的工作原理。现在，让我们看看如何使用 DRF 将基于令牌的身份验证集成到 Django 中。

## 向 Django 项目添加 DRF 基于令牌的身份验证

在 DRF 中，身份验证是在视图执行之前作为第一步检查的。重要的是要理解，与 Django 基于会话的身份验证不同，DRF 身份验证*不是通过中间件实现的*。相反，DRF 在关联的视图执行开始时检查身份验证。DRF 主要采用这种方法，以确保 DRF 是一个即插即用系统，可以轻松地与其他视图使用的其他身份验证机制共存。例如，Django Admin Views 使用原生的 Django 基于会话的身份验证。

与 Django 一样，DRF 在请求成功身份验证后也会将 `Auth User` 对象附加到 `request.user` 属性。如果请求没有任何身份验证属性，则它会将 `Anonymous` user 附加到 `request.user`。现在让我们学习将 DRF 令牌集成到我们的 Django 项目的步骤：

1.  DRF 包有一个内部应用，可在我们的数据库中创建 `Token` 表。我们需要在 Django 设置的已安装应用中添加 `rest_framework.authtoken`，以将 `authtoken` 应用附加到我们的项目中：

```
INSTALLED_APPS = [
    ...
    'rest_framework.authtoken'
]
```

2.  运行 `python manage.py migrate` Django 迁移命令以在我们的数据库中创建 `Token` 表。
3.  将默认身份验证类添加到配置设置中。

DRF 在视图执行开始前评估身份验证，因此我们可以为每个视图定义身份验证类。但如果开发者忘记添加身份验证，这可能会导致安全漏洞。因此，建议附加默认身份验证类。如果我们需要更改特定视图的身份验证，则始终可以在 *view* 定义中覆盖身份验证。

仅仅将身份验证附加到视图是不够的；我们还需要告诉 DRF 给定视图的权限类型是什么。默认情况下，我们应该为所有视图添加 `IsAuthenticated`，以便所有视图始终可由经过身份验证的 user 访问，除非明确说明。





我们需要在 `settings.py` 文件中添加以下配置，以为我们所有的视图启用身份验证和权限：

```
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ]
}
```

从现在起，所有映射到 DRF 视图的请求都将经过 DRF 基于令牌的身份验证，我们将检查发出请求的<|message|>用户是否已登录。

### 4. 我们需要创建一个 `login` 视图，让用户可以获取用于身份验证的令牌。请注意，我们需要为 `login` 视图显式添加 `permission_classes` 为 `AllowAny`，否则我们会收到 `401` 错误：

```
from rest_framework.authtoken.models import Token
from rest_framework.permissions import AllowAny
from django.contrib.auth import authenticate
@api_view(['POST'])
@permission_classes([AllowAny])
def login(request)
  username = request.DATA['username']
  password = request.DATA['password']
  user = authenticate(username=username, password=password)
  if not user:
    return Response(status='401')
  token = Token.objects.get_or_create(user=user)
  return Response(data={"token": token.key})
```

### 5. 生成令牌后，我们需要在对 Django 服务器发出的每个后续请求中附加该令牌。将令牌添加到 `Authorization` 头键中；该键应以 `Token` 字符串文字为前缀，两个字符串之间用空格分隔：

```
Authorization: Token <token key>
```

然后，`curl` 请求应如下所示：

```
curl -X GET http://127.0.0.1:8000/api/example/ -H 'Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b'
```

我们现在已为项目配置了 DRF 基于令牌的身份验证。有了它，对我们 DRF 视图发出的任何请求都将始终检查基于令牌的身份验证以及请求是否已通过身份验证。DRF 还支持并行身份验证和权限类；如果我们想附加更多身份验证方法，则将它们添加为列表。例如，如果我们还希望默认的 Django 基于会话的身份验证适用于我们的 DRF 视图，则可以添加以下内容：

```
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
        'rest_framework.authentication.SessionAuthentication',
    ]
}
```

使用多个 Django 身份验证可以帮助我们为 Django 项目支持多种身份验证。DRF 将尝试使用第一个身份验证类对请求进行身份验证。如果成功，则不会检查其他类并将用户附加到 `request.user` 属性，否则，它将继续尝试后续的身份验证类。

Read more

DRF 权限类在内部使用 Django 组和权限框架来支持模型级权限。我们不会深入探讨 Django 权限框架的细节以及如何在 DRF 项目中使用它。你可以查看 DRF 文档中提到的示例以了解更多详情：[`www.django-rest-framework.org/api-guide/permissions/#api-reference`](https://www.django-rest-framework.org/api-guide/permissions/#api-reference)。

DRF 还提供了一个接口来根据我们的需求创建自定义身份验证和权限类。DRF 的官方文档提到了扩展这些类的逐步流程。我们不会对这些类深入探讨，我会让大家为此目的遵循官方教程。

Read more

对于自定义身份验证类，请遵循此链接：[`www.django-rest-framework.org/api-guide/authentication/#custom-authentication`](https://www.django-rest-framework.org/api-guide/authentication/#custom-authentication)。

对于自定义权限类，请遵循官方文档中提到的教程：[`www.django-rest-framework.org/api-guide/permissions/#custom-permissions`](https://www.django-rest-framework.org/api-guide/permissions/#custom-permissions)。

现在，让我们来看一下 DRF 基于令牌身份验证的几个局限性。

## 理解 DRF 基于令牌身份验证的局限性

DRF 基于令牌的身份验证是一个相当简单的实现。对于任何基本应用程序，DRF 都能完美运行，但当你开始构建具有身份验证的复杂功能时，就会发现其中的缺陷。让我们了解 DRF 基于令牌身份验证的一些不足之处：

*   **不支持多设备登录**：由于 DRF 为每个用户创建一个令牌，如果用户在多个设备上登录，则我们将在每个设备上使用相同的令牌。如果用户在不受信任的设备上登录，这是一个安全问题。用户唯一的选择是删除旧令牌；这将使用户从所有设备上注销。
*   **未加密的令牌存储在数据库中**：每当创建新令牌时，DRF 都会以原始格式保存令牌；因此如果数据库 ever 被攻破，黑客将能够访问所有用户。
*   **无过期时间**：DRF 会跟踪每个令牌的创建时间，但它不支持令牌过期。在现代应用程序中，提供过期和刷新令牌功能非常重要。
*   **每个请求都需要数据库查找**：对于每个请求，都将涉及一次数据库查询，以根据头中设置的令牌获取用户信息。
*   **无法通过令牌向前端发送任何数据**：现代应用程序使用令牌在不进行额外 API 调用的情况下检索有关用户的基本信息。
*   **不支持社交登录**：用户希望通过 Google、GitHub 等社交登录平台进行一键式顺畅登录。

这些是使用 DRF 基于令牌身份验证的几个常见缺点。虽然没有简单的解决方案可以解决所有这些用例，但我们可以查看一些第三方包，它们提供了不同的方法来解决这些问题。

# 了解第三方基于令牌的身份验证包

在本节中，我们将对可用于 Django 和 DRF 以解决 DRF 基于令牌身份验证挑战的不同可用包进行高层次概述。我们不会深入探讨以下包的实现和集成；相反，我们将使开发人员了解他们可以轻松使用以解决问题而存在的开源包。

## django-rest-knox

`django-rest-knox` 包提供了多设备登录和会话管理支持。它的架构与 DRF 基于令牌的身份验证类似，但它解决了几个问题，例如以加密格式保存令牌并为已签发的令牌提供过期时间。

Read more

有关更多详细信息，请查看官方文档：[`jazzband.github.io/django-rest-knox/`](https://jazzband.github.io/django-rest-knox/)。

### djangorestframework-simplejwt

`JSON Web Token` `JWT` 是一种现代的基于令牌的身份验证标准，在开发者中相当流行（阅读更多内容请访问 [`jwt.io/introduction`](https://jwt.io/introduction)）。JWT 的优点是它不需要数据库查找即可进行验证，并且我们可以将信息添加到已签发的 JWT 令牌本身中，客户端可以利用这些信息获取有关用户的信息。`djangorestframework-simplejwt` 包有助于在 DRF 项目中启用 JWT 身份验证。

Read more





官方文档详细介绍了如何将 JWT 集成到 Django 项目中：[`django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html`](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html)

有许多包，例如 `django-allauth`、`djoser`、`django-rest-knox`、`dj-rest-auth` 等，提供了一套不同的认证机制，但在选择任何包之前，我们应该意识到该包带来了哪些功能，以及自己构建这些功能是否合理。这是一个需要权衡取舍的问题，因为一旦我们使用第三方包进行认证，之后再逆转这个决定将会非常困难。

认证的最后一个重要方面是社交登录。社交登录是当今现代 Web 应用中最常见的登录形式。让我们学习如何为我们的 Django 项目集成社交登录。

# 将社交登录集成到 Django 和 DRF

社交登录对于任何面向客户端的应用都至关重要。在当今世界，用户不想填写冗长的注册表单，也不想为每个网站记住密码；这些障碍都可以通过社交登录来解决。有多个服务，例如 Google、Facebook、Twitter 和 GitHub，可以接入我们的网站以启用社交登录。这些平台各自的实现细节不同，实现起来可能很棘手。我建议你使用 `python-social-auth` 第三方包将社交登录集成到 Django 应用中。

我们不会深入 `python-social-auth` 的实现细节（[`python-social-auth.readthedocs.io/en/latest/configuration/django.html`](https://python-social-auth.readthedocs.io/en/latest/configuration/django.html)），因为有多种类型的服务提供商，例如 Google、Facebook、GitHub 等。你最好阅读各自平台的集成文档。

# 总结

在本章中，我们学习了 Django 如何开箱即用地提供认证。Django 还借助 Django 权限和组提供了授权支持。RBAC 是现代应用所需的关键安全功能之一，Django 已开箱即用地提供。

我们还学习了如何使用 DRF 和 Django 使用基于令牌的认证。基于令牌的认证对于非浏览器客户端很有用，例如 Android、iOS 和物联网设备。社交登录是当今现代应用必备的功能，我们讨论了如何在 Django 中使用 `python-social-auth` 来集成社交登录。

在下一章中，我们将学习如何在 Django 应用中实现缓存、日志记录和节流，以及它们带来的额外优势。
