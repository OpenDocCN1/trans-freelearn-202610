

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

在 [*第 4 章*](#B18867_04.xhtml#_idTextAnchor102) 中，在探索 Django Admin 时，我们使用 Django `superuser` 在管理面板中导航；因此不需要权限。但随着项目进入生产环境，我们希望根据其用例为每个用户提供受限访问。例如，支持代理只需要对所有支付模型具有 *只读* 访问权限，并且不应访问任何其他数据库。这类 RBAC 可以轻松通过 Django Admin 实现。

当我们以超级用户身份登录 Django 管理面板时，我们将能够看到身份验证和授权 UI，如 *图 5**.1* 所示。`Groups` 和 `Users` 部分帮助我们为 Django 应用程序实现 RBAC：

![Figure 5.1: Django admin panel showing AUTHENTICATION and AUTHORIZATION](image/B18867_05_01.jpg) 

图 5.1：显示 AUTHENTICATION 和 AUTHORIZATION 的 Django 管理面板

默认情况下，Django 为每个模型对象创建权限对象。这些权限对象在 Django Admin 中用于检查使用 Django Admin 的特定用户是否有授权执行请求的模型操作。如果我们在 Django Admin UI 中打开 auth `User` 对象，我们将能够看到附加到特定用户的所有组和用户权限。例如，如 *图 5**.2* 所示，我们可以看到可附加到用户的全部用户权限。Django 为项目中所有模型创建权限对象，具有 `add`、`change`、`delete` 和 `view` 等不同操作类型。我们可以选择特定模型，然后将所需权限附加到用户对象：

![Figure 5.2: User object view in Django Admin showing all the groups and user permissions attached to the given user](image/B18867_05_02.jpg) 

图 5.2：Django Admin 中用户对象视图，显示附加到给定用户的所有组和用户权限

尽管我们始终可以将单个权限附加到用户，但最好始终使用组来为任何用户授予权限，以便所有权限都可以通过组集中管理。Django `permissions` 帮助我们创建模型操作的精细控制，而 Django `groups` 是权限的集合，有助于我们创建角色。例如，支持代理可能只需要访问查看 author 模型和 blog 模型，因此我们将创建一个名为 `Support Agent` 的 Django 组并添加 author 和 blog 模型的只读权限，如 *图 5**.3* 所示：

![Figure 5.3: Creating a new Django Support Agent group](image/B18867_05_03.jpg) 

图 5.3：创建新的 Django Support Agent 组

一旦我们创建了 Django `Support Agent` 组，我们将其附加到用户，如 *图 5**.4* 所示。这将允许用户通过 Django Admin 以只读方式访问 `author` 和 `blog` 表：

![Figure 5.4: Attaching the Support Agent group to a Django user](image/B18867_05_04.jpg) 

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

