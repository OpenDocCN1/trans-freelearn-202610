

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

更多详情，请查阅官方文档了解如何在 DRF 中创建自定义权限类：[https://www.django-rest-framework.org/api-guide/permissions/#custom-permissions](https://www.django-rest-framework.org/api-guide/permissions/#custom-permissions%20)

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

现代 Web 应用不再仅限于浏览器。事实上，*超过 55%* 的互联网流量来自移动设备（根据这份 *Cloudflare* 报告：[https://radar.cloudflare.com/traffic?range=28d](https://radar.cloudflare.com/traffic?range=28d)）。Django 最初是在 18 多年前构建的，当时浏览器更流行，但在当今世界，移动设备和应用的数量超过了浏览器，这需要不同类型的身份验证。DRF 开箱即支持多种身份验证类型：

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

