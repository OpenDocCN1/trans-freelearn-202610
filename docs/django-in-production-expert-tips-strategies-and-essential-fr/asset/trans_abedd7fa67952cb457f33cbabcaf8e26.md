

```
class BlogGetUpdateFilterView(generics.ListAPIView):
    serializer_class = BlogSerializer
    filter_backends = [filters.OrderingFilter]
    ordering_fields = ['title']
```

同样，我们可以使用 `SearchFilter` 后端。官方文档提到了如何使用不同过滤器的基本示例，以及如何创建自定义过滤器后端。

Read more

DRF 还支持创建自定义过滤器并扩展现有过滤器。官方文档提供了示例，并对开发者如何最好地使用此功能进行了适当的解释：

[https://www.django-rest-framework.org/api-guide/filtering/#api-guide](https://www.django-rest-framework.org/api-guide/filtering/#api-guide)。

在本节中，我们学习了如何将 DRF 序列化器与 DRF 视图集成。我们还看到了 DRF 如何开箱即用地支持过滤器，以及我们如何使用它们来改进开发。

# Summary

在本章中，我们学习了如何使用 DRF 序列化器。首先，我们探索了基本 DRF 序列化器的概念，以及 DRF 如何使用 Renderer 和 Parser 将数据从 JSON 转换为 Python 原生数据类型（反之亦然）。然后，我们学习了如何使用 DRF `ModelSerializer` 与 Django 模型。

DRF 序列化器开箱即用地提供数据验证支持。在本章中，我们学习了使用序列化器集成字段级和对象级验证的不同方法。我们还学习了如何将 DRF 序列化器集成到与 Django 关联的字段中并使用嵌套序列化器。

最后，我们学习了如何将 DRF 序列化器与 DRF 视图集成，以全面了解序列化器在整体应用程序开发中的工作方式。

在下一章中，我们将学习如何使用 Django 管理后台以及如何使用 Django 管理命令。

# Exploring Django Admin and Management Commands

在 [*Chapter 3*](#B18867_03.xhtml#_idTextAnchor067) 中，我们学习了如何将 DRF 序列化器与视图集成，以及客户端应用程序如何与 Django 视图交互。我们的用户将使用客户端应用程序通过 REST API 执行 CRUD 操作。但在实际应用中，我们需要创建一些额外的接口，让管理员用户执行某些 CRUD 操作。Django 开箱即用地提供了一个管理界面来满足此用例。

在本章中，我们将学习如何使用 Django Admin，它提供了很大的灵活性来配置管理界面，以支持不同的业务用例。我们将学习如何探索这些配置，以及如何使用管理命令更好地与框架交互（例如执行数据库迁移或启动开发服务器）。Django 还提供了扩展这些命令的接口，以便我们可以为自定义用例创建自定义管理命令。我们也将对此进行探讨。

本章将涵盖以下主题：

*   Exploring Django Admin
*   Customizing Django Admin
*   Optimizing Django Admin for production
*   Creating custom management commands

## Technical requirements

在本章中，我们将学习如何使用 Django Admin 面板。Django Admin 与 Django 模型紧密配合，因此您需要对 Django 模型有良好的理解。

本章中使用的代码片段可以在本书的 GitHub 仓库中找到，地址为 [https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter04](https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter04)。

## Exploring Django Admin

对于任何应用程序，我们都需要某种管理 UI 来执行管理任务。Django Admin 开箱即用地提供了这种强大的功能。但在查看界面之前，像其他任何管理界面一样，我们需要用户登录凭据来访问 Django Admin。

### Creating a superuser in Django

Django 为我们提供了内置的 `createsuperuser` 管理命令来创建新的管理员用户。一旦我们在终端中运行此命令，我们将看到一个引导向导，帮助我们创建 Django 超级用户：

```
python manage.py createsuperuser
```

按照说明并输入提供的数据后，我们将获得一个 Django 超级用户。

Django authentication and authorization

Django 开箱即用地提供身份验证和授权支持。当我们在 Django 中创建超级用户时，我们将使用默认的 Django 身份验证系统。我们将在 [*Chapter 5*](#B18867_05.xhtml#_idTextAnchor126) 中讨论 Django 身份验证和授权的细节。

一旦我们创建了超级用户，我们就可以运行 Django 服务器以登录管理界面。默认情况下，Django Admin 在主 `urls.py` 文件中配置为 `/admin` 路由。现在，让我们学习如何配置 Django Admin 界面以及 Django 为我们与 Django Admin 协作提供的所有功能。

### Understanding the Django Admin interface

当我们在浏览器中访问 `http://127.0.0.1:8000/admin` 时，我们将被重定向到管理员登录页面。UI 界面由 Django 自动生成，开箱即用。一旦我们输入之前创建的超级用户管理员的凭据，我们将能够登录管理界面，如 *Figure 4**.1* 所示：

![Figure 4.1: Default Django Admin interface](image/B18867_04_01.jpg)

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

![Figure 4.2: The BLOG section in the Django Admin UI](image/B18867_04_02.jpg)

Figure 4.2: The BLOG section in the Django Admin UI

现在，让我们使用更多配置来配置 Django Admin，以改善 Admin 页面的用户体验。首先，让我们看看博客模型的列表页面在 Django Admin 中是什么样子的。*Figure 4**.3* 显示了博客模型的 Django Admin 列表页面：

![Figure 4.3: The Django Admin UI for the blog model](image/B18867_04_03.jpg)

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

官方 Django 文档展示了我们可以用来自定义 Django Admin 体验的所有开箱即用选项：[https://docs.djangoproject.com/en/stable/ref/contrib/admin/](https://docs.djangoproject.com/en/stable/ref/contrib/admin/)。

