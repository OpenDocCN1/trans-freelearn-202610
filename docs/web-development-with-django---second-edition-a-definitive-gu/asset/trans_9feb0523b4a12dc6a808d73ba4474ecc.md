

存在不同的认证方案，例如 **basic authentication**、**session authentication**、**token authentication**、**remote user authentication**，以及各种第三方认证解决方案。对于本章的范围以及我们的 Bookr 应用，我们将使用 token 认证。

> **Note**
> 有关所有认证方案的进一步阅读，请参阅 https://www.django-rest-framework.org/api-guide/authentication 上的官方文档。

## Token-based authentication

基于令牌的认证通过为用户生成一个唯一的令牌来工作，以换取用户的用户名和密码。一旦令牌生成，它将被存储在数据库中以供进一步参考，并在每次登录时返回给用户。

此令牌是用户独有的，用户随后可以使用它来授权他们发出的每个 API 请求。基于令牌的认证消除了在每个请求中传递用户名和密码的需求。它更安全，非常适合客户端-服务器通信，例如基于 JavaScript 的 Web 客户端通过 REST API 与后端应用程序交互。

一个例子是 ReactJS 或 AngularJS 应用程序通过 REST API 与 Django 后端交互。

如果您正在开发移动应用程序以通过 REST API 与后端服务器交互，也可以使用相同的架构，例如 Android 或 iOS 应用程序通过 REST API 与 Django 后端交互。

## Exercise 12.05 – Implementing token-based authentication for Bookr APIs

在本练习中，您将为 bookr 应用程序的 API 实现基于令牌的认证：

- 1. 打开 bookr/settings.py 文件并将 rest_framework.authtoken 添加到 INSTALLED_APPS：

```
INSTALLED_APPS = [
  'django.contrib.admin',
  'django.contrib.auth',
  'django.contrib.contenttypes',
  'django.contrib.sessions',
  'django.contrib.messages',
  'django.contrib.staticfiles',
  'rest_framework',
  'rest_framework.authtoken',
  'reviews']
```

- 2. 由于 authtoken 应用有相关的数据库更改，请在命令行/终端中按如下方式运行 migrate 命令：

```
python manage.py migrate
```

- 3. 打开 bookr/reviews/api_views.py 文件，删除任何现有代码，并替换为以下内容：

```
from django.contrib.auth import authenticate
from rest_framework import viewsets
from rest_framework.authentication import
    TokenAuthentication
from rest_framework.authtoken.models import Token
from rest_framework.pagination import
    LimitOffsetPagination
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
from rest_framework.status import HTTP_404_NOT_FOUND,
    HTTP_200_OK
from rest_framework.views import APIView
```

您可以在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter12/Exercise12.05/bookr/reviews/api_views.py 找到此文件的完整代码。

在这里，我们定义了一个名为 Login 的视图。此视图的目的是允许用户获取（如果尚不存在则创建）一个令牌，以便他们可以使用它来对 API 进行认证。

我们重写此视图的 post 方法，因为我们希望自定义用户向我们发送数据（即他们的登录详细信息）时的行为。首先，我们使用 Django auth 库中的 authenticate 方法来检查用户名和密码是否正确。如果正确，则我们将拥有一个用户对象。如果不正确，我们将返回 HTTP 404 错误。如果我们确实拥有有效的用户对象，那么我们只需获取或创建令牌并将其返回给用户。

- 4. 接下来，让我们将 authentication class 添加到 BookViewSet。这意味着当用户尝试访问此 viewset 时，它将要求他们使用基于令牌的认证进行认证。请注意，可以包含不同接受的认证方法的列表，而不仅仅是一种。我们还添加了 permissions_classes 属性，它只是使用 DRF 的内置类来检查给定用户是否有权限查看此模型中的数据：

```
class BookViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]
```

> **Note**
> 
> 上述代码（高亮显示）与您在 GitHub 上看到的代码不匹配，因为我们在第 9 步中稍后会对其进行修改。

- 5. 打开 `bookr/reviews/urls.py` 文件并在 URL 模式中添加以下路径：

```
path(
    'api/login', api_views.Login.as_view(),
    name='login'
)
```

- 6. 保存文件并等待应用程序重启，或使用 `python manage.py runserver` 命令手动启动服务器。然后使用 `http://127.0.0.1:8000/api/login` URL 访问应用程序。您的屏幕应如下所示：

![](img/4840a43efd335abd10b93708225692d6_575_0.png)

`/api/login` 上的 API 仅为 POST 消息；因此显示 Method GET not allowed。

7. 接下来，在 Content 字段中输入以下片段并点击 POST：

```
{
"username": "Peter",
"password": "testuserpassword"
}
```

您需要将其替换为您数据库中账户的实际用户名和密码。现在您可以看到为用户生成的令牌。这是我们需要用来访问 BookSerializer 的令牌：

![](img/4840a43efd335abd10b93708225692d6_576_0.png)

图 12.8：为用户生成的令牌

8. 尝试使用我们之前创建的 API 在 http://127.0.0.1:8000/api/books/ 访问图书列表。请注意，您现在不被允许访问它。这是因为此 viewset 现在要求您使用令牌进行认证。

相同的 API 可以使用命令行上的 curl 访问：

```
curl -X GET http://127.0.0.1:8000/api/books/

{"detail":"Authentication credentials were not provided."}
```

由于未提供令牌，因此显示 `Authentication credentials were not provided` 消息：

![](img/4840a43efd335abd10b93708225692d6_577_0.png)

图 12.9：提示未提供认证详细信息的消息

请注意，如果您使用的是 Windows 10，请将上述命令中的 `curl` 替换为 `curl.exe` 并从命令提示符执行。

要将 Authorization 令牌（在第 7 步获得）作为 header 传递，您可以使用以下命令（Windows 用户可以将 `curl` 替换为 `curl.exe`）：

```
```bash
curl -X GET http://127.0.0.1:8000/api/books/ -H "Authorization: Token cd5dafa1d4361df1502652d260eed3ddb55c64f1"
```
```

> **Note**
> 
> 在粘贴此命令之前，请确保您已将令牌替换为您在执行本练习第 7 步时获得的令牌。它与我们在此处显示的不同。

上述命令现在应返回图书列表：

```
```json
[{"title":"Advanced Deep Learning with Keras","publication_date":"2018-10-31","isbn":"9781788629416","publisher":{"name":"Packt Publishing","website":"https://www.packtpub.com/","email":"info@packtpub.com"},"rating":4,"reviews":[{"content":"A must read for all","date_created":... (truncated)
```
```

此操作确保只有应用程序的现有用户才能访问并获取所有图书的集合。

9. 在继续之前，将 BookViewSet 上的 authentication 和 permission classes 设置为空字符串。未来的章节将不会使用这些认证方法，并且为了简单起见，我们假设未经认证的用户可以访问我们的 API：

```
class BookViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    authentication_classes = []
    permission_classes = []
```

在本练习中，我们在 Bookr 应用中实现了基于令牌的认证。我们创建了一个登录视图，允许我们检索给定已认证用户的令牌。这随后使我们能够通过在请求中将令牌作为 header 传递，从命令行发出 API 请求。

总体而言，在本节中，我们了解了不同类型的认证，然后在与 REST API 协作时详细了解了令牌认证和授权。

## Summary

本章介绍了 REST API，这是大多数现实世界 Web 应用程序中的基本构建块。这些 API 促进了后端服务器与 Web 浏览器之间的通信，因此对于您作为 Django Web 开发人员的成长至关重要。我们学习了如何序列化数据库中的数据，以便通过 HTTP 请求传输。接下来，我们了解了 DRF 为我们提供的各种选项来简化我们编写的代码，利用模型本身的现有定义。我们还介绍了 ViewSets 和路由器，并看到了它们如何通过结合多个视图的功能来进一步压缩代码。最后，我们了解了认证和授权，并为图书评论应用实现了基于令牌的认证。

在下一章中，我们将通过学习如何生成 CSV、PDF 和其他二进制文件类型来扩展 Bookr 的用户功能。

