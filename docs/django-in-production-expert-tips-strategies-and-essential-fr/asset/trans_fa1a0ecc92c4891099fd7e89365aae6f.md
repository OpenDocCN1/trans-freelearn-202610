

*   为端点使用逻辑嵌套结构对于清理 API 接口和维护良好的信息架构非常重要。例如，要获取特定博客的所有评论，API 应该是 `GET /blogs/<blog id>/comments`。

*   对 API 进行版本控制很重要，因为它有助于在不破坏新系统中契约的情况下支持旧系统。例如 `/v1/blogs/` 和 `/v2/blogs`。我们将在 *Using API versioning* 部分稍后了解更多关于此的内容。

*   服务器应根据操作发送适当的 HTTP 响应状态码，并附带消息正文（如果适用）。以下是一些最常用的 HTTP 状态码：
    *   `2xx`：用于任何成功。例如，`200` 用于任何请求成功返回数据，`201` 用于创建新条目，等等。
    *   `3xx`：用于任何重定向。
    *   `4xx`：用于任何错误。例如，使用 `400` 表示请求错误，使用 `404` 表示请求的数据未找到。
    *   `5xx`：当服务器因任何意外请求而崩溃或服务器不可用时。

*   服务器必须接受并以 JSON 响应进行响应。API 不支持其他数据类型，例如纯文本、XML 等。

## 使用 DRF 创建 REST API 的最佳实践

DRF 是一个框架，可以帮助我们更快地创建 REST 端点。遵循最佳实践的同时编写可扩展且可维护的代码是开发人员的责任。让我们看看使用 DRF 可以实现的一些最佳实践。

### 使用 API 版本控制

创建 API 版本可能是与客户端合作时最重要的事情之一，而这些客户端的更新不在你的控制之下。一个例子是开发移动应用。一旦终端用户安装了集成了给定 API 的移动应用，我们就必须在终端用户更新移动应用版本以使用更新的 API 之前支持该 API。

在创建端点时，开发人员应考虑所有可能将来的需求以及所有边界情况。然而，就像无法预测未来一样，开发人员也无法总是预见到当前 API 设计可能需要重新设计。重新设计意味着破坏客户端和服务器之间的契约。这就是 API 版本控制重要性显现的时候。经过良好版本控制的 API 将在不破坏任何现有客户端的情况下实现新契约。

实现 API 版本控制有多种方式：

*   `Accept` 头版本控制：由于版本通过 `Accept` 头传递，因此每当创建新版本时，客户端无需在每次创建新版本时更新任何端点：
```
GET /bookings/ HTTP/1.1
Host: example.com
Accept: application/json; version=1.0
```

*   `URL` 路径版本控制：API 版本通过 URL 路径模式传递。这是使用最广泛的 API 版本控制方法之一，因为它提供了更好的可见性：
```
GET /v1/bookings/ HTTP/1.1
Host: example.com
Accept: application/json
```

*   查询参数版本控制：URL 中的查询参数包含版本。在 URL 路径版本控制之后，这是第二常见的版本控制方法，因为它具有更清晰的界面和更好的可发现性：
```
GET /something/?version=1.0 HTTP/1.1
Host: example.com
Accept: application/json
```

*   主机名版本控制：这涉及使用子域在主机名中传递 API 版本。这种类型的版本控制用于将整个服务迁移到新版本而不是单个端点时：
```
GET /bookings/ HTTP/1.1
Host: v1.example.com
Accept: application/json
```

DRF 开箱即支持所有四种 API 版本控制方法，并且如果需要，还提供创建自定义 API 版本逻辑的选项。我们将主要探索 `URLPathVersioning`，因为它是通过 DRF 实现版本控制的最简单和最流行的方法之一。

在 URL 路径版本控制中，API 版本在 URL 路径中传递，这使得客户端和服务器都可以轻松识别。要在 DRF 中集成 `URLPathVersioning`，请在 Django `config/settings.py` 文件中添加以下内容：
```
REST_FRAMEWORK = {
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.URLPathVersioning'
}
```

现在，我们必须将版本添加到 URL 路径中。重要的是将 URL 参数命名为 `<version>`，因为 DRF 默认期望它是 `<version>`。这里，`<version>` 是 URL 的模式，这意味着任何匹配此模式的 URL 都将被链接到视图。

重要提示

要了解有关 `urlpatterns` 的更多信息，请访问 [https://docs.djangoproject.com/en/stable/topics/http/urls/](https://docs.djangoproject.com/en/stable/topics/http/urls/)。

建议在 URL 开头添加 `<version>`，因此让我们在主 `config/urls.py` 文件中这样做：
```
urlpatterns = [
    path('admin/', admin.site.urls),
    path('<version>/demo-app-version/', include('demo_app.urls'))
]
```

一旦我们使用 `<version>` 配置了 URL，我们就可以尝试创建一个新视图并在视图中检索版本。将以下代码添加到你的 `demo_app/urls.py` 文件中：
```
from django.urls import path
from demo_app import views
urlpatterns = [
    path('hello-world/', views.hello_world),
    path('demo-version/', views.demo_version),
]
```

我们将在视图中检索 API 版本并在响应中返回版本：
```
@api_view(['GET'])
def demo_version(request, *args, **kwargs):
    version = request.version
    return Response(data={
        'msg': f'You have hit {version} of demo-api'
    })
```

现在，当我们打开 `http://127.0.0.1:8000/v1/demo-app-version/demo-version/` 时，我们应该能够看到以下屏幕：

![Figure 1.7: Output showing which version we have hit for the given API](image/B18867_01_07.jpg)

Figure 1.7: Output showing which version we have hit for the given API

如果我们将 URL 更改为 `http://127.0.0.1:8000/v9/demo-app/demo-version/`，那么我们会看到它返回 v9。v9 可能尚未发布，因此这可能会让访问该端点的终端用户感到困惑。为了解决这个问题，我们将看看如何自定义 DRF 的版本类，以便我们可以添加约束来帮助我们设计更好的应用程序。

#### 使用 DRF 中的自定义版本类

让我们看看如何扩展 DRF 提供的 `URLPathVersioning` 类来解决我们刚刚提出的问题。首先，创建一个名为 `demo_app/custom_versions.py` 的文件。此文件将为每个视图包含一个自定义版本类，以及一个用于尚未具有多个版本的视图的默认类：
```
from rest_framework.versioning import URLPathVersioning
class DefaultDemoAppVersion(URLPathVersioning):
    allowed_versions = ['v1']
    version_param = 'version'
class DemoViewVersion(DefaultDemoAppVersion):
    allowed_versions = ['v1', 'v2', 'v3']
class AnotherViewVersion(DefaultDemoAppVersion):
    allowed_versions = ['v1', 'v2']
```

让我们看看前面的代码做了什么：

*   `DefaultDemoAppVersion` 类可用于 `demo_app` 中创建的所有视图。它具有 `allowed_versions` 属性，列出了在使用此基于类的视图时可在 URL 路径中使用的允许版本。`version_param` 是我们用于定义版本的 URL 路径参数名称；它可以是任何名称，具体取决于你如何命名参数，但在我们的例子中，我们使用的是 `<version>`，它在 `config/urls.py` 文件中使用。此类将默认用于 demo 应用中创建的所有视图，直到添加新版本后，我们将创建单独的类，如下所示。

