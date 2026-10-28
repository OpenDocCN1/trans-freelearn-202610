## 12 构建 REST API

在上一章中，我们学习了模板和基于类的视图。这些概念极大地帮助我们扩展了可以在前端（用户的网络浏览器中）向用户提供的功能范围。然而，这还不足以构建一个现代网络应用程序。Web 应用通常使用完全独立的库（如 ReactJS 或 AngularJS）构建前端。这些库提供了构建动态用户界面的强大工具，但不会直接与我们的 Django 后端代码或数据库通信。前端代码只是在网络浏览器中运行，无法直接访问后端服务器上的任何数据。因此，我们需要创建一种方式，使这些应用程序能够“对话”我们的后端代码。在 Django 中，实现这一目标的最佳方法之一是使用 REST API。

**API** 用于促进不同软件之间的交互，它们通过**超文本传输协议（HTTP）**进行通信。这是服务器和客户端之间通信的标准协议，也是信息在网络上传送的基础。API 以 HTTP 格式接收请求并发送响应。

本章介绍 **REST API** 和 **Django REST framework（DRF）**。您将首先为 Bookr 项目实现一个简单的 API。接下来，您将学习模型实例的序列化，这是向 Django 应用程序前端传递数据的关键步骤。最后，您将探索不同类型的 API 视图，包括函数式视图和基于类的视图。

在本章的使用场景中，API 将帮助我们促进 Django 后端与前端 JS 代码之间的交互。例如，假设我们要创建一个允许用户向 Bookr 数据库添加新书的前端应用程序。用户的网络浏览器会向我们的 API 发送一条消息（HTTP 请求），表示他们要为新书创建一个条目，并可能在该消息中包含一些书籍详细信息。我们的服务器会发送回一个响应，报告书籍是否成功添加。然后，网络浏览器便能够向用户显示其操作的结果。

在本章中，我们将涵盖以下主题：

- 理解 REST API
- Django REST framework

到本章结束时，您将能够实现自定义 API 端点，包括基于令牌的身份验证。

### 技术要求

本章的所有代码文件可在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter12 找到。

### 理解 REST API

大多数现代 Web API 都可以归类为 **REST** API。REST API 只是一种专注于在数据库服务器和前端客户端之间通信和同步对象*状态*的 API 类型。

例如，假设您正在一个已登录账户的网站上更新您的详细信息。当您进入账户详细信息页面时，网络服务器会告诉您的浏览器与您账户相关的各种详细信息。当您更改该页面上的值时，浏览器会将更新后的详细信息发送回网络服务器，并告诉它在数据库中更新这些详细信息。如果操作成功，网站将向您显示一条确认消息。

这是所谓的前后端系统之间**解耦**架构的一个非常简单示例。解耦提供了更大的灵活性，并使更新或更改架构中的组件更加容易。所以，假设您想创建一个新的前端网站。在这种情况下，只要您的新前端构建为发出与旧前端相同的 API 请求，您根本不需要更改后端代码。

REST API 是*无状态的*，这意味着客户端和服务器都不会存储任何状态来进行通信。每次发出请求时，数据都会被处理，并发送回响应，而协议无需存储任何中间数据。这意味着 API 是隔离地处理每个请求的。它不需要存储有关会话本身的信息。这与有状态协议（如**传输控制协议（TCP）**）形成对比，后者在其生命周期内维护有关会话的信息。

因此，**RESTful Web 服务**，顾名思义，是用于执行一组任务的 REST API 集合。例如，如果我们为 Bookr 应用程序开发一组 REST API 来执行某一组任务，我们可以称之为 RESTful Web 服务。接下来，我们将开始使用 DRF。

### Django REST framework

DRF 是一个开源的 Python 库，可用于为 Django 项目开发 REST API。DRF 内置了大多数必要的功能，可帮助为任何 Django 项目开发 API。在本章中，我们将使用它为 Bookr 项目开发 API。在下一节中，我们将安装和配置 DRF。

### 安装与配置

要在与 PyCharm 一起设置的虚拟环境中安装 djangorestframework，请按照以下步骤操作：

- 1. 在您的终端应用程序或命令提示符中输入以下代码：

```
pip install djangorestframework
```

- 2. 接下来，打开 settings.py 文件，并将 rest_framework 添加到 INSTALLED_APPS，如以下代码片段所示：

```
INSTALLED_APPS = [
    "bookr_admin.apps.BookrAdminConfig",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "rest_framework",
    "reviews", ]
```

您现在可以开始使用 DRF 创建您的第一个简单 API 了。

### 函数式 API 视图

在第 3 章“URL 映射、视图和模板”中，我们学习了简单的函数式视图，它们接收一个请求并返回一个响应。我们可以使用 DRF 编写类似的函数式视图。但是，请注意，基于类的视图更常用，接下来将介绍。函数式视图只需将以下装饰器添加到普通视图上即可创建，如下所示：

```
from rest_framework.decorators import api_view

@api_view
def my_view(request):
    ...
```

该装饰器将函数式视图转换为 APIView DRF 的子类。这是将现有视图作为 API 一部分包含进来的快速方法。在下一节中，我们将利用目前学到的知识，使用 DRF 创建一个简单的 REST API。





+   4. 使用 `python manage.py runserver` 命令启动 Django 服务，并访问 `http://127.0.0.1:8000/api/first_api_view/` 发出你的第一次 API 请求。你的屏幕应如图 12.1 所示：

![](img/4840a43efd335abd10b93708225692d6_557_0.png)

图 12.1 – 带有书籍数量的第一个 API 视图

调用此 URL 端点对 API 端点发出了默认的 GET 请求，该请求返回了一个 JSON 键值对 (`"num_books": 0`)。此外，请注意 DRF 如何提供一个漂亮的界面来查看和与 API 交互。

+   5. 我们也可以使用 Linux `curl`（客户端 URL）命令发送 HTTP 请求，如下所示：

```
```bash
curl http://127.0.0.1:8000/api/first_api_view/
{"num_books":18}
```
```

或者，如果你使用的是 Windows 10，你也可以从命令提示符使用 `curl.exe` 发出等效的 HTTP 请求，如下所示：

```
```bash
curl.exe http://127.0.0.1:8000/api/first_api_view/
```
```

在本练习中，我们学习了如何使用 DRF 和一个简单的函数视图创建 API 视图。现在我们将看看使用序列化器在数据库中存储的信息与 API 返回的内容之间进行转换的更优雅的方法。

```
```python
class BookSerializer(serializers.Serializer):
    title = serializers.CharField()
    publication_date = serializers.DateField()
    isbn = serializers.CharField()
    publisher = PublisherSerializer()
```
```

这里，第一行从 rest_framework 模块导入了序列化器。

在导入之后，我们定义了两个类，PublisherSerializer 和 BookSerializer。顾名思义，它们分别是 Publisher 和 Book 模型的序列化器。这两个序列化器都是 serializers.Serializer 的子类，我们为每个序列化器定义了字段类型，例如 CharField、URLField、EmailField 等。

查看 bookr/reviews/models.py 文件中的 Publisher 模型。Publisher 模型具有 name、website 和 email 属性。因此，要序列化 Publisher 对象，我们需要在序列化器类中使用 name、website 和 email 属性，我们已在 PublisherSerializer 中相应地定义了它们。

同样，对于 Book 模型，我们在 BookSerializer 中将 title、publication_date、isbn 和 publisher 定义为期望的属性。由于 publisher 是 Book 模型的外键，我们使用了 PublisherSerializer 作为 publisher 属性的序列化器。

+   3. 打开 `bookr/reviews/api_views.py`，删除任何预先存在的代码，并添加以下代码：

```
```python
from rest_framework.decorators import api_view
from rest_framework.response import Response

from .models import Book
from .serializers import BookSerializer


@api_view()
def all_books(request):
    books = Book.objects.all()
    book_serializer = BookSerializer(books, many=True)
    return Response(book_serializer.data)
```
```

在第二行，我们从 serializers 模块导入了新创建的 BookSerializer。

然后我们添加一个函数视图 all_books（与之前的练习一样）。该视图接收包含所有书籍的查询集，然后使用 BookSerializer 对其进行序列化。序列化器类还接收一个参数 many=True，这表明 books 对象是查询集或多个对象的列表。请记住，序列化会将 Python 对象转换为 JSON 可序列化格式，如下所示：

```
[OrderedDict([('title', 'Advanced Deep Learning with Keras'), ('publication_date', '2018-10-31'), ('isbn', '9781788629416'), ('publisher', OrderedDict([('name', 'Packt Publishing'), ('website', 'https://www.packtpub.com/'), ('email', 'info@packtpub.com')]))]), OrderedDict([('title', 'Hands-On Machine Learning for Algorithmic Trading'), ('publication_date', '2018-12-31'), ('isbn', '9781789346411'), ('publisher', OrderedDict([('name', 'Packt Publishing'), ('website', 'https://www.packtpub.com/'), ('email', 'info@packtpub.com')]))]) ...
```

4. 打开 bookr/reviews/urls.py，删除 first_api_view 的先前示例路径，并添加 all_books 路径，如以下代码所示：

```
from django.urls import path
from . import views, api_views

urlpatterns = [
    path(
        'api/all_books/',
        api_views.all_books, name='all_books'
    ),
    ...
]
```

当 URL 中遇到 api/all_books/ 路径时，此新添加的路径会调用 all_books 视图函数。

与常规视图一样，DRF 提供了一组通用视图，使得编写基于类的视图变得更加简单。通用视图是针对创建 API 时所需的一些最常见操作而设计的。DRF 提供的一些通用视图有 ListAPIView、RetrieveAPIView 等。在练习 12.02《创建 API 视图以显示书籍列表》中，我们的函数视图负责创建对象的查询集然后调用序列化器。等效地，我们可以使用 ListAPIView 来完成同样的事情：

```
class AllBooks(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

这里，对象的查询集被定义为类属性。将查询集传递给序列化器由 ListAPIView 上的方法处理。

## 模型序列化器

在练习 12.02《创建 API 视图以显示书籍列表》中，我们的序列化器定义如下：

```
class BookSerializer(serializers.Serializer):
    title = serializers.CharField()
    publication_date = serializers.DateField()
    isbn = serializers.CharField()
    publisher = PublisherSerializer()
```

然而，我们的 Book 模型看起来像这样（注意模型和序列化器的定义有多相似）：

```
class Book(models.Model):
    """A published book."""

    title = models.CharField(max_length=70,
        help_text="The title of the book.")
    publication_date = models.DateField(
        verbose_name="Date the book was published.")
    isbn = models.CharField(max_length=20,
        verbose_name="ISBN number of the book.")
    publisher = models.ForeignKey(Publisher, on_delete=models.CASCADE)
    contributors = models.ManyToManyField("Contributor",
        through="BookContributor")
```

我们更希望不指定 title 必须是 serializers.CharField()。如果序列化器只需查看 title 在模型中的定义并能推断出要使用的序列化器字段类型，那就更容易了。

这就是模型序列化器的用武之地。它们通过利用模型上字段的定义来提供创建序列化器的快捷方式。不是指定 title 应该使用 CharField 进行序列化，我们只需告诉模型序列化器我们想要包含 title，它就会使用 CharField 序列化器，因为模型上的 title 字段也是 CharField。

例如，假设我们想要为 models.py 中的 Contributor 模型创建一个序列化器。不是为每个字段指定应使用哪些序列化器类型，我们可以给它一个字段名称列表，让它推断其余部分：

```
from rest_framework import serializers

from .models import Contributor

class ContributorSerializer(serializers.ModelSerializer):
    class Meta:
        model = Contributor
        fields = ['first_names', 'last_names', 'email']
```

在下面的练习中，我们将看到如何使用模型序列化器来避免前述类中的代码重复。

## 练习 12.03 – 创建基于类的 API 视图和模型序列化器

在本练习中，你将创建基于类的视图来显示所有书籍的列表，同时使用模型序列化器：

+   1. 打开 bookr/reviews/serializers.py 文件，删除任何预先存在的代码，并用以下代码替换它：

```
from rest_framework import serializers
from .models import Book, Publisher

class PublisherSerializer(
    serializers.ModelSerializer):
    class Meta:
```

```
model = Publisher
fields = ['name', 'website', 'email']

class BookSerializer(serializers.ModelSerializer):
    publisher = PublisherSerializer()

    class Meta:
        model = Book
        fields = ['title', 'publication_date', 'isbn',
            'publisher']
```

这里，我们包含了两个模型序列化器类，PublisherSerializer 和 BookSerializer。这两个类都继承自 serializers.ModelSerializer 父类。我们不需要指定每个字段如何被序列化；相反，我们可以简单地传递一个字段名称列表，字段类型将从 models.py 中的定义中推断出来。

虽然在 fields 中提及字段对于模型序列化器来说已经足够，但在某些特殊情况下，例如这种情况，我们可能需要自定义字段，因为 publisher 字段是外键。因此，我们必须使用 PublisherSerializer 来序列化 publisher 字段。

2. 接下来，打开 bookr/reviews/api_views.py，删除任何预先存在的代码，并添加以下代码：

```
from rest_framework import generics

from .models import Book
from .serializers import BookSerializer

class AllBooks(generics.ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

这里，我们使用 DRF 基于类的 ListAPIView 而不是函数视图。这意味着书籍列表被定义为类属性，我们不必编写一个直接处理请求并调用序列化器的函数。来自上一步的书籍序列化器也被导入并分配为该类的属性。

打开 bookr/reviews/urls.py 文件并修改 /api/all_books API 路径以包含新的基于类的视图，如下所示：

```
urlpatterns = [
    path(
        'api/all_books/',
        api_views.AllBooks.as_view(),
        name='all_books'
    ),
```





由于我们使用的是基于类的视图，因此必须使用类名以及 as_view() 方法。

+   3. 完成上述所有修改后，等待 Django 服务重启或使用 python manage.py runserver 命令启动服务器，然后在浏览器中打开 API 地址 http://127.0.0.1:8000/api/all_books/。你应该会看到类似图 12.3 的内容：

![](img/4840a43efd335abd10b93708225692d6_565_0.png)

图 12.3：all_books 端点中显示的图书列表

与我们在练习 12.02《创建 API 视图以显示图书列表》中看到的情况类似，这是一个图书评论应用中所有图书的列表。在本练习中，我们使用模型序列化器简化了代码，并使用通用的基于类的 ListAPIView 返回数据库中图书的列表。在下一节中，你将实现一个 API 端点来返回顶级贡献者的列表。

## 活动 12.01 – 为顶级贡献者页面创建 API 端点

假设你的团队决定创建一个网页来展示数据库中的顶级贡献者（作者、合著者和编辑）。他们决定聘请外部开发人员使用 ReactJS 创建一个应用。为了与 Django 后端集成，开发人员需要一个提供以下内容的端点：

+   - 数据库中所有贡献者的列表
+   - 对于每个贡献者，他们所贡献的所有图书的列表
+   - 对于每个贡献者，他们所贡献的图书数量
+   - 对于他们所贡献的每本书，他们在该书中的角色

最终的 API 视图应如下所示：

![](img/4840a43efd335abd10b93708225692d6_566_0.png)

图 12.4：顶级贡献者端点

要执行此任务，请执行以下步骤：

+   1. 向 `Contributor` 类添加一个方法，返回所做的贡献数量。
+   2. 添加 `ContributionSerializer`，序列化 `BookContribution` 模型。
+   3. 添加 `ContributorSerializer`，序列化 `Contributor` 模型。
+   4. 添加 `ContributorView`，使用 `ContributorSerializer`。
+   5. 在 `urls.py` 中添加一个模式以启用对 `ContributorView` 的访问。

> **注意**
> 
> 本活动的解决方案可在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions 上找到。

## 使用 ViewSets 简化代码

我们已经看到如何使用基于类的通用视图优化代码并使其更简洁。**ViewSets** 和 **routers** 帮助我们进一步简化代码。正如名称所示，ViewSets 是一组表示在单个类中的视图。例如，我们使用 `AllBooks` 视图返回应用中所有图书的列表，并使用 `BookDetail` 视图返回单本图书的详细信息。使用 ViewSets，我们可以将这两个类合并为一个类。

DRF 还提供了一个名为 `ModelViewSet` 的类。该类不仅结合了前面讨论中提到的两个视图（列表和详情），而且还允许你创建、更新和删除模型实例。实现所有这些功能所需的代码可能就像指定序列化器和查询集一样简单。例如，允许你管理用户模型的所有这些操作的视图可以简洁地定义如下：

```
class UserViewSet(viewsets.ModelViewSet):
    serializer_class = UserSerializer
    queryset = User
```

最后，DRF 提供了一个 `ReadOnlyModelViewSet` 类。这是前面 `ModelViewSet` 的简化版本。它是相同的，只是它只允许你列出和检索特定用户。你无法创建、更新或删除记录。在下一节中，我们将学习有关路由器的内容。

## 使用路由器和 Viewsets 进行 URL 配置

路由器与 viewset 一起使用时，会自动为 viewset 创建所需的 URL 端点。这是因为单个 viewset 在不同的 URL 下访问。例如，在前面的 UserViewSet 中，你将在 /api/users/ URL 访问用户列表，并在 /api/users/123 URL 访问特定用户记录，其中 123 是该用户记录的主键。以下是如何在之前定义的 UserViewSet 的上下文中使用路由器的简单示例：

```
from rest_framework import routers
router = routers.SimpleRouter()
router.register(r'users', UserViewSet)
urlpatterns = router.urls
```

现在，让我们尝试在一个简单的练习中结合路由器和 ViewSets 的概念。

## 练习 12.04 – 使用 ViewSets 和路由器

在本练习中，我们将组合现有视图以创建 viewset 并为 viewset 创建所需的路由：

+   1. 打开 bookr/reviews/serializers.py 文件，删除预先存在的代码，并添加以下代码片段：

```
from django.contrib.auth.models import User
from django.utils import timezone
from rest_framework import serializers
from rest_framework.exceptions import
    NotAuthenticated, PermissionDenied
from .models import Book, Publisher, Review
from .utils import average_rating
class PublisherSerializer(
    serializers.ModelSerializer):
```

你可以在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter12/Exercise12.04/bookr/reviews/serializers.py 找到完整的代码片段。

这里，我们向 BookSerializer 添加了两个新字段，即 reviews 和 rating。这些字段有趣的地方在于其背后的逻辑是在序列化器本身上定义为方法。这就是为什么我们使用 serializers.SerializerMethodField 类型来设置序列化器类属性的原因。

+   2. 打开 `bookr/reviews/api_views.py` 文件，删除预先存在的代码，并添加以下内容：

```
```python
from rest_framework import viewsets
from rest_framework.pagination import (
    LimitOffsetPagination)

from .models import Book, Review
from .serializers import (BookSerializer,
    ReviewSerializer)

class BookViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Book.objects.all() serializer_class
    = BookSerializer

class ReviewViewSet(viewsets.ModelViewSet):
    queryset = Review.objects.order_by('-
    date_created')
    serializer_class = ReviewSerializer
    pagination_class = LimitOffsetPagination
    authentication_classes = []
```
```

这里，我们删除了 AllBook 和 BookDetail 视图，并用 BookViewSet 和 ReviewViewSet 取代。在第一行，我们从 `rest_framework` 导入 ViewSets 模块。BookViewSet 类是 ReadOnlyModelViewSet 的子类，这确保视图仅用于 GET 操作。

+   3. 接下来，打开 `bookr/reviews/urls.py` 文件，删除以 `api/` 开头的最初两个 URL 模式，然后添加以下（高亮显示）代码：

```
```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from . import views, api_views
router = DefaultRouter()
router.register(r'books', api_views.BookViewSet)
router.register(r'reviews', api_views.ReviewViewSet)

urlpatterns = [
    path(
        'api/', include((router.urls, 'api'))
    ),
    path(
```
```

4. 保存所有文件，并在 Django 服务重启后（或手动使用 python manage.py runserver 命令启动），转到 http://127.0.0.1:8000/api/books/ URL 获取所有图书的列表。你应该在 API explorer 中看到以下视图：

![](img/4840a43efd335abd10b93708225692d6_571_0.png)

图 12.5：/api/books 路径上的图书列表

5. 你也可以使用 http://127.0.0.1:8000/api/books/1/ URL 访问特定图书的详细信息。在这种情况下，它将返回主键为 1 的图书的详细信息（如果它存在于你的数据库中）：

![](img/4840a43efd335abd10b93708225692d6_572_0.png)

图 12.6：“Advanced Deep Learning with Keras” 的图书详情

在本练习中，我们看到了如何使用 ViewSets 和路由器将列表和详情视图组合到单个 viewset 中。使用 ViewSets 使我们的代码更一致、更符合惯例，使与其他开发人员协作变得更容易。当与独立的前端应用集成时，这变得尤为重要。在下一节中，我们将简要了解不同类型的身份验证并为图书评论应用实现基于令牌的身份验证。

## 实现身份验证

正如我们在第 9 章《会话和身份验证》中学到的，验证应用用户的身份非常重要。良好的做法是只允许在应用中注册的用户登录并访问应用信息。同样，对于 REST API，我们也需要在传递任何信息之前设计一种验证和授权用户的方式。例如，假设 Facebook 网站发出 API 请求以获取帖子的所有评论列表。如果该端点没有身份验证，你就可以以编程方式获取你想要的任何帖子的评论。他们显然不希望允许这样做，因此需要实现某种身份验证。





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





## 生成 CSV、PDF 及其他二进制文件

到目前为止，我们已经学习了 Django 框架的各个方面，并探索了如何使用 Django 构建具有我们想要的所有功能和自定义的 Web 应用程序。

假设在构建 Web 应用程序时，我们需要进行一些分析和准备一些报告。我们可能需要分析用户的人口统计数据，了解平台的使用情况，或生成可输入机器学习系统以寻找模式的数据。我们希望我们的网站以表格格式显示分析结果的一部分，并以详细的图表和图形显示其他结果。此外，我们希望允许用户导出报告，并在 Jupyter Notebook 和 Excel 等应用程序中进一步查看。

随着我们逐步学习本章，我们将学习如何将这些想法付诸实践，并在我们的 Web 应用程序中实现功能，允许我们将记录导出为结构化格式，例如通过使用逗号分隔值（CSV）文件或 Excel 文件的表格。我们还将学习如何允许用户生成我们 Web 应用程序中存储的数据的可视化表示，并以 PDF 格式导出，以便于快速参考时轻松分发。

让我们从学习如何在 Python 中处理 CSV 文件开始。学习这项技能将帮助我们创建允许读者导出数据以进行进一步分析的功能。

在本章中，我们将涵盖以下主题：

- 在 Python 内部处理 CSV 文件
- 使用 Python 的 csv 模块
- 在 Python 中处理 Excel 文件
- 在 Python 中处理 PDF 文件
- 在 Python 中玩转图表
- 将可视化与 Django 集成

## 技术要求

本章的完整代码文件可以在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter13 找到

## 在 Python 内部处理 CSV 文件

我们可能需要导出应用程序中的数据，有几个原因。其中一个原因可能涉及分析该数据——例如，我们可能需要了解在应用程序上注册的用户的人口统计数据，或提取应用程序使用模式。我们可能还需要了解我们的应用程序对用户的工作情况，以便设计未来的改进。此类用例要求数据采用易于使用和分析的格式。在这里，CSV 文件格式派上了用场。

CSV 是一种方便的文件格式，可用于以行列格式快速从应用程序导出数据。CSV 文件通常使用简单的分隔符分隔数据，用于区分一列和另一列，并使用换行符来指示表中新记录（或行）的开始。

Python 在其标准库中对处理 CSV 文件有很好的支持，这要归功于 csv 模块。此支持使读取、解析和写入 CSV 文件成为可能。让我们看看如何利用 Python 提供的 csv 模块来处理 CSV 文件并从中读取和写入数据。

## 使用 Python 的 csv 模块

Python 的 csv 模块允许我们与 CSV 格式的文件进行交互，这只是一种文本文件格式——也就是说，存储在 CSV 文件中的数据是人类可读的。

csv 模块要求在应用 csv 模块提供的方法之前先打开文件。让我们看看如何开始最基本的操作——从 CSV 文件读取数据。

### 从 CSV 文件读取数据

从 CSV 文件读取数据非常容易，并且包含以下步骤：

首先，我们必须通过运行以下脚本打开文件：

```
csv_file = open('path to csv file')
```

这里，我们使用 Python 的 open() 方法读取文件，该方法需要文件的完全限定名称。这应该打开，以便将其作为参数传递给方法。

然后，我们必须使用 csv 模块的 reader 方法从文件对象读取数据：

```
import csv
csv_data = csv.reader(csv_file)
```

2. 一旦文件下载完成，打开它并查看其内容。你会意识到该文件包含一组用逗号分隔的值，每个不同的记录都在自己的行上：

![](img/4840a43efd335abd10b93708225692d6_582_0.png)

图 13.1 – market_cap CSV 文件的内容

3. 一旦文件下载完成，你可以编写第一段代码。为此，在 CSV 文件下载的同一目录中创建一个名为 csv_reader.py 的新文件，并添加以下代码：

```
import csv

def read_csv(filename):
    """Read and output the details of CSV file."""
    try:
        with open(filename, newline='') as csv_file:
            csv_reader = csv.reader(csv_file)
            for record in csv_reader:
                print(record)
    except (IOError, OSError) as file_read_error:
```

```
print("Unable to open the csv file. Exception: {}".format(file_read_error))

if __name__ == '__main__':
    read_csv('market_cap.csv')
```

让我们尝试理解你在前面的代码片段中实现的内容。

在导入 csv 模块后，为了保持代码模块化，你创建了一个名为 read_csv() 的新方法，它接受一个参数——要从中读取数据的文件名：

```
try:
    with open(filename, newline='') as csv_file:
```

现在，如果你不熟悉前面代码片段中显示的打开文件的方法，这也称为 try-with-resources 方法。在这种情况下，封装在 with 块范围内的任何代码块都将有权访问文件对象，并且一旦代码退出 with 块的范围，文件将自动关闭。

**重要说明**

将文件 I/O 操作封装在 try-except 块中是一个好习惯，因为文件 I/O 可能因多种原因而失败，向用户显示堆栈跟踪不是一个好选择。

reader() 方法返回一个 reader 对象，你可以对其进行迭代以访问值，正如我们在 从 CSV 文件读取数据 部分所看到的：

```
for record in csv_reader:
    print(record)
```

完成此操作后，你必须编写入口点方法，从该方法开始执行你的代码，通过调用 read_csv() 方法并传递要读取的 CSV 文件的名称：

```
if __name__ == '__main__':
    read_csv('market_cap.csv')
```

4. 这样，你就完成了并准备好解析你的 CSV 文件。你可以通过在终端或命令提示符中运行你的 Python 文件来做到这一点，如下所示：

```
python3 csv_reader.py
```

代码执行后，你应该会看到以下输出：

```
sbadhwar@sbadhwar-mn1 Exercise13.01 % python3 csv_reader.py
['id', 'stock_symbol', 'market_cap']
['1', 'HOTRW', 'n/a']
['2', 'MSFG', '$861.23M']
['3', 'CERC', '$7.18M']
['4', 'BOOT', '$184.25M']
['5', 'PMO', '$481M']
['6', 'FISI', '$443.54M']
['7', 'JASO', '$301.6M']
['8', 'PIO', '$191.36M']
['9', 'UBP^G', 'n/a']
['10', 'BVN', '$2.85B']
['11', 'MMM', '$127.36B']
['12', 'GATX', '$2.54B']
['13', 'JPM^A', 'n/a']
['14', 'ACCO', '$1.24B']
['15', 'HBI', '$8.23B']
['16', 'Y', '$9.37B']
['17', 'DHX', '$128.99M']
['18', 'AGNCB', '$9.12B']
['19', 'BT', '$37.72B']
['20', 'GGP^A', 'n/a']
['21', 'XRAY', '$14.6B']
['22', 'ANCB', '$62.74M']
['23', 'MLP', '$386.9M']
['24', 'AEG', '$10.51B']
['25', 'CELGZ', 'n/a']
```

图 13.2：CSV 读取程序的输出

通过这样，你现在知道如何读取 CSV 文件内容了。此外，正如你从练习 13.01 – 使用 Python 读取 CSV 文件 的输出中所看到的，单个行的输出以列表的形式表示。

现在，让我们看看如何使用 Python csv 模块创建新的 CSV 文件。

## 使用 Python 写入 CSV 文件

在上一节中，我们探讨了如何使用 Python 中的 csv 模块读取 CSV 格式文件的内容。现在，让我们学习如何将 CSV 数据写入文件。

写入 CSV 数据遵循与从 CSV 文件读取类似的方法，只是存在一些细微差异。以下步骤概述了将数据写入 CSV 文件的过程：

通过运行以下脚本以写入模式打开文件：

```
csv_file = open('path to csv file', 'w')
```





获取一个 CSV 写入器对象，它可以帮助我们以正确的 CSV 格式写入数据。这可以通过调用 csv 模块的 `writer()` 方法来完成，该方法返回一个写入器对象，可用于将兼容 CSV 格式的数据写入 CSV 文件：

```
```python
csv_writer = csv.writer(csv_file)
```
```

一旦写入器对象可用，我们就可以开始写入数据。这由写入器对象的 `writerow()` 方法实现。`write_row()` 方法接收一个值列表并将其写入 CSV 文件。列表本身表示一行，列表中的值表示列的值：

```
```python
record = ['value1', 'value2', 'value3']
csv_writer.writerow(record)
```
```

如果您想在一次调用中写入多条记录，也可以使用 CSV 写入器的 `writerows()` 方法。`writerows()` 方法的行为与 `writerow()` 方法类似，但接收一个列表的列表，可以一次性写入多行：

```
```python
records = [['value11', 'value12', 'value13'], ['value21',
'value22', 'value23']]
csv_writer.writerows(records)
```
```

记录写入后，我们可以关闭 CSV 文件：

```
```python
csv_file.close()
```
```

现在，让我们将所学应用到下一个练习中，并实现一个程序来帮助我们将值写入 CSV 文件。

## 练习 13.02 – 使用 Python 的 csv 模块生成 CSV 文件

在本练习中，您将使用 Python csv 模块创建新的 CSV 文件：

+   1. 创建一个名为 `csv_writer.py` 的新文件，在其中编写 CSV 写入器的代码。在此文件中，添加以下代码：

```
```python
import csv


def write_csv(filename, header, data):
    """Write the provided data to the CSV file.

    :param str filename: The name of the file to which
        the data should be written
    :param list header: The header for the columns in
        CSV file
    :param list data: The list of list mapping the
        values to the columns
    """
```
```

```
try:
    with open(filename, 'w') as csv_file:
        csv_writer = csv.writer(csv_file)
        csv_writer.writerow(header)
        csv_writer.writerows(data)
except (IOError, OSError) as csv_file_error:
    print("Unable to write the contents to csv
          file. Exception:
          {}".format(csv_file_error))
```

有了这段代码，您现在应该能够轻松创建新的 CSV 文件。现在，逐步来看，我们在这段代码中试图做什么。

我们定义了一个名为 write_csv() 的新方法，它接收三个参数：要写入数据的文件名（filename）、用作标题的列名列表（header），以及包含需要映射到各个列的数据的列表的列表（data）：

```
def write_csv(filename, header, data):
```

现在参数就绪，下一步是打开需要写入数据的文件并将其映射到对象：

```
with open(filename, 'w') as csv_file:
```

文件打开后，我们执行三个主要步骤：

+   首先，我们使用 csv 模块的 writer() 方法获得一个新的 CSV 写入器对象，并将其传递给保存我们打开文件引用的文件处理程序：

```
csv_writer = csv.writer(csv_file)
```

+   下一步涉及使用 CSV 写入器的 writerow() 方法将数据集的标题字段写入文件：

```
csv_writer.writerow(header)
```

+   写入标题后，最后一步是将数据写入 CSV 文件的各个列。为此，我们可以使用 csv 模块的 writerows() 方法一次写入多行：

```
csv_writer.writerows(data)
```

## 重要说明

虽然前面的步骤概述了写入 CSV 文件的详细版本，以提供对 CSV 文件应如何结构化的底层工作原理的更好清晰度，但我们本可以将写入标题和数据的步骤合并为一行代码，通过将标题列表作为数据列表的第一个元素，并调用 writerows() 方法并将数据列表作为参数。

2. 一旦我们创建了可以将提供的数据写入 CSV 文件的方法，我们必须编写入口点调用的代码，并在其中设置 header、data 和 filename 字段的值，最后调用我们之前定义的 write_csv() 方法：

```
if __name__ == '__main__':
    header = ['name', 'age', 'gender']
    data = [['Richard', 32, 'M'], ['Mumzil', 21, 'F'],
            ['Melinda', 25, 'F']]
    filename = 'sample_output.csv'
    write_csv(filename, header, data)
```

3. 现在代码就绪，执行我们刚刚创建的文件，看看它是否创建了 CSV 文件。要执行它，运行以下命令：

```
python3 csv_writer.py
```

执行完成后，您会看到在执行命令的同一目录中创建了一个新文件。打开该文件后，其内容应类似于以下图所示：

![](img/4840a43efd335abd10b93708225692d6_587_0.png)

图 13.3：CSV 写入器 sample_output.csv 的输出

这应导致以下输出：

```
```text
{'id': '1', 'stock_symbol': 'HOTRW', 'market_cap': 'n/a'}
{'id': '2', 'stock_symbol': 'MSFG', 'market_cap': '$861.23M'}
{'id': '3', 'stock_symbol': 'CERC', 'market_cap': '$7.18M'}
{'id': '4', 'stock_symbol': 'BOOT', 'market_cap': '$184.25M'}
{'id': '5', 'stock_symbol': 'PMO', 'market_cap': '$481M'}
{'id': '6', 'stock_symbol': 'FISI', 'market_cap': '$443.54M'}
{'id': '7', 'stock_symbol': 'JASO', 'market_cap': '$301.6M'}
{'id': '8', 'stock_symbol': 'PIO', 'market_cap': '$191.36M'}
{'id': '9', 'stock_symbol': 'UBPGA', 'market_cap': 'n/a'}
{'id': '10', 'stock_symbol': 'BVN', 'market_cap': '$2.85B'}
{'id': '11', 'stock_symbol': 'MMM', 'market_cap': '$127.36B'}
{'id': '12', 'stock_symbol': 'GATX', 'market_cap': '$2.54B'}
{'id': '13', 'stock_symbol': 'JPM^D', 'market_cap': 'n/a'}
```
```

图 13.4：使用 DictReader 的输出

正如前一幅图所示，各个行被映射为字典中的键值对。要访问行中的这些各个字段，我们可以使用以下方法：

```
```python
print(record.get('stock_symbol'))
```
```

这将为我们提供来自各个记录的 stock_symbol 字段的值。

同样，您也可以使用 DictWriter() 接口以字典形式操作 CSV 文件。为了演示这一点，让我们看一下来自 *练习 13.02 – 使用 Python 的 csv 模块生成 CSV 文件* 的 write_csv() 方法，并按如下方式修改它：

```
```python
def write_csv(filename, header, data):
    """Write the provided data to the CSV file.

    :param str filename: The name of the file to which the
                         data should be written
    :param list header: The header for the columns in CSV
                        file
    :param list data: The list of dicts mapping the values
                      to the columns
    """
    try:
        with open(filename, 'w') as csv_file:
            csv_writer = csv.DictWriter(csv_file,
                                        fieldnames=header)
            csv_writer.writeheader()
            csv_writer.writerows(data)
    except (IOError, OSError) as csv_file_error:
        print("Unable to write the contents to CSV file.
              Exception: {}".format(csv_file_error))
```
```

在前面的代码中，我们将 csv.writer() 替换为 csv.DictWriter()，它提供了一个类似字典的接口来与 CSV 文件交互。DictWriter() 还接收一个 fieldnames 参数，用于在写入前映射 CSV 文件中的各个列。

接下来，要写入此标题，请调用 writeheader() 方法，它会将 fieldname 标题写入 CSV 文件。

最后一次调用涉及 writerows() 方法，它接收一个字典列表并将其写入 CSV 文件。为了使代码正确工作，您还需要将 data 列表修改为如下所示：

```
data = [{'name': Richard, 'age': 32, 'gender': 'M'},
    {'name': Mumzil', 'age': 21, 'gender':'F'},
    {'name': 'Melinda', 'age': 25, 'gender': 'F'}]
```

有了这些，您就有足够的知识在 Python 中处理 CSV 文件。

通过本节，我们学习了如何使用标准库提供的 csv 模块在 Python 中处理 CSV 文件。

由于我们正在讨论如何处理表格数据，特别是将其读写到文件，让我们看一下由最受欢迎的表格数据编辑器之一使用的更知名的文件格式之一 – Microsoft Excel。

## 在 Python 中使用 Excel 文件

Microsoft Excel 是记账和表格记录管理领域世界闻名的软件。同样，随着 Excel 引入的 XLSX 文件格式也得到了快速和广泛的采用，现在已得到所有主要产品供应商的支持。

您会发现 Microsoft Excel 及其 XLSX 格式在许多公司的市场营销和销售部门被大量使用。假设，对于这样一家公司的市场营销部门，您正在 Django 中构建一个 Web 门户，用于跟踪用户购买的产品。它还显示有关购买的数据，例如购买时间和购买地点。市场营销和销售团队计划使用这些数据来生成潜在客户或创建相关广告。

市场营销和销售团队也大量使用 Excel。您可能希望以 Excel 原生的 XLSX 格式导出 Web 应用程序中的可用数据。

很快，我们将了解如何使我们的网站与 XLSX 格式协同工作。但在此之前，让我们先快速了解一下二进制文件格式的使用。




## 数据导出的二进制文件格式

到目前为止，我们主要处理的是文本数据，以及如何从文本文件中读写这些数据。但通常，基于文本的格式并不够用。例如，假设你想导出一张图片或一个图表。你如何将图片或图表表示为文本，又如何读写这些图片呢？

为了帮助我们应对这些情况，二进制文件格式应运而生，它可以帮助我们读写各种丰富多样的数据。所有商业操作系统都原生支持文本和二进制文件格式，而且毫不奇怪的是，Python 提供了最通用的处理二进制数据文件的实现之一。一个简单的例子是 open 命令，你可以用它来指定要以哪种格式打开文件：

```
file_handler = open('path to file', 'rb')
```

这里的 b 表示二进制。

从本节开始，我们现在将处理如何使用二进制文件，并用它们来表示和导出来自 Django Web 应用的数据。我们将要讨论的第一种格式是 XLSX 文件格式，这种格式因 Microsoft Excel 而广受欢迎。

那么，让我们深入了解如何使用 Python 处理 XLSX 文件。

## 使用 XlsxWriter 包处理 XLSX 文件

在本节中，我们将进一步了解 XLSX 文件格式，并理解如何使用 XlsxWriter 包来处理它。

### XLSX 文件

XLSX 文件是用于存储表格数据的二进制文件。任何实现了对该格式支持的软件都可以读取这些文件。XLSX 格式将数据划分为两个逻辑分区：

- **工作簿**：每个 XLSX 文件都称为一个工作簿，应该包含与特定领域相关的数据集。在图 13.5 中，Examplefile.xlsx 是一个工作簿（1）：

![](img/4840a43efd335abd10b93708225692d6_592_0.png)

图 13.5：Excel 中的工作簿和工作表

- **工作表**：在每个工作簿中，可以有一个或多个工作表，它们以表格格式存储不同但逻辑相关的数据集。在图 13.5 中，**Sheet1** 和 **Sheet2** 是两个工作表（2）。

处理 XLSX 格式时，我们通常操作的就是这两个单元。如果你了解关系数据库，可以将工作簿视为数据库，将工作表视为表。

现在，让我们试着理解如何在 Python 中开始处理 XLSX 文件。

### XlsxWriter Python 包

Python 没有通过其标准库提供对 XLSX 文件的原生支持。但得益于 Python 生态系统中庞大的开发者社区，很容易找到几个可以帮助我们管理与 XLSX 文件交互的包。这一类别中一个流行的包是 **XlsxWriter**。

XlsxWriter 是一个由开发者社区积极维护的包，为与 XLSX 文件交互提供支持。该包提供了许多有用的功能，支持创建和管理工作簿，以及各个工作簿中的工作表。你可以在终端或命令提示符中运行以下命令来安装它：

```
pip install XlsxWriter
```

安装完成后，你可以按如下方式导入 xlsxwriter 模块：

```
import xlsxwriter
```

那么，让我们看看如何使用 XlsxWriter 包开始创建 XLSX 文件。

### 创建工作簿

要开始处理 XLSX 文件，我们首先需要创建它们。XLSX 文件也称为工作簿，可以通过调用 xlsxwriter 包中的 Workbook 类来创建，如下所示：

```
workbook = xlsxwriter.Workbook(filename)
```

调用 Workbook 类会打开一个由 filename 参数指定的二进制文件，并返回一个工作簿实例，该实例可用于进一步创建工作表和写入数据。

### 创建工作表

在开始向 XLSX 文件写入数据之前，我们需要先创建一个工作表。这可以通过调用上一节中获得的工作簿对象的 add_worksheet() 方法轻松完成：

```
worksheet = workbook.add_worksheet()
```

add_worksheet() 方法会创建一个新工作表，将其添加到工作簿中，并返回一个与该工作表相对应的 Python 对象，通过该对象我们可以向工作表写入数据。

### 向工作表写入数据

一旦获得了对工作表的引用，我们就可以通过调用工作表对象的 write 方法开始向其写入数据，如下所示：

```
worksheet.write(row_num, col_num, col_value)
```

如你所见，write() 方法接受三个参数：行号（row_num）、列号（col_num），以及属于 [row_num, col_num] 对的数据，由 col_value 表示。可以重复调用此方法，将多个数据项插入工作表中。

### 向工作簿写入数据

一旦所有数据都已写入，要完成写入数据集的收尾并干净地关闭 XLSX 文件，你必须对工作簿调用 close() 方法：

```
workbook.close()
```

此方法会写入文件缓冲区中可能存在的任何数据，并最终关闭工作簿。现在，让我们利用这些知识来实现我们自己的代码，帮助我们向 XLSX 文件写入数据。

> 重要说明
> 本章无法涵盖 XlsxWriter 包提供的所有方法和功能。如需更多信息，你可以阅读官方文档：https://xlsxwriter.readthedocs.io/contents.html。

## 练习 13.03 – 在 Python 中创建 XLSX 文件

在本练习中，我们将使用 XlsxWriter 包创建一个新的 Excel（XLSX）文件，并从 Python 向其中添加数据：

- 1. 在系统上安装 XlsxWriter 包，请在终端应用或命令提示符中运行以下命令：
  
      `pip install XlsxWriter`
  
      命令执行完成后，你的系统上就会安装好该包。
      安装好该包后，我们就可以开始编写用于创建 Excel 文件的代码了。

- 2. 创建一个名为 xlsx_demo.py 的新文件，并向其中添加以下代码：

```
import xlsxwriter

def create_workbook(filename):
    """Create a new workbook on which we can work."""
    workbook = xlsxwriter.Workbook(filename)
    return workbook
```

在上面的代码片段中，我们创建了一个新函数，它将帮助我们创建一个可用于存储你的数据的新工作簿。创建好新工作簿后，下一步是创建一个工作表，它为我们提供了组织存储在 XLSX 工作簿中的数据所需的表格格式。

3. 工作簿创建好后，通过向你的 xlsx_demo.py 文件中添加以下代码片段来创建一个新工作表：

```
def create_worksheet(workbook):
    """Add a new worksheet in the workbook."""
    worksheet = workbook.add_worksheet()
    return worksheet
```

在上面的代码片段中，我们使用 XlsxWriter 包提供的工作簿对象的 add_worksheet() 方法创建了一个新工作表。该工作表将用于写入对象的数据。

4. 下一步是创建一个辅助函数，帮助我们将数据按行号和列号定义的表格格式写入工作表。为此，请将以下代码片段添加到你的 xlsx_writer.py 文件中：

```
def write_data(worksheet, data):
    """Write data to the worksheet."""
    for row in range(len(data)):
        for col in range(len(data[row])):
            worksheet.write(row, col, data[row][col])
```

在上面的代码片段中，我们创建了一个名为 write_data() 的新函数，它接受两个参数：需要写入数据的工作表对象，以及需要写入工作表的、由列表的列表表示的数据对象。该函数遍历传递给它的数据，然后将数据写入其所属的行和列。

现在，所有核心方法都已实现，我们可以添加一个方法，帮助干净地关闭工作簿对象，以便数据能够写入文件，而不会发生任何文件损坏。为此，请在 xlsx_demo.py 文件中实现以下代码片段：

```
def close_workbook(workbook):
    """Close an opened workbook."""
    workbook.close()
```

最后一步是集成我们在前面步骤中实现的所有方法。为此，请在 xlsx_demo.py 文件中创建一个新的入口点方法：

```
if __name__ == '__main__':
    data = [['John Doe', 38], ['Adam Cuvver', 22],
            ['Stacy Martin', 28], ['Tom Harris', 42]]
    workbook = create_workbook('sample_workbook.xlsx')
    worksheet = create_worksheet(workbook)
    write_data(worksheet, data)
    close_workbook(workbook)
```





在前面的代码片段中，我们创建了一个数据集，我们希望以列表的列表形式将其写入 XLSX 文件。完成之后，我们获得了一个新的工作簿对象，该对象将用于创建 XLSX 文件。在此工作簿对象内部，我们创建了一个工作表以以行列格式组织数据，将数据写入工作表，并关闭工作簿以将数据持久化到磁盘。

5. 现在，让我们看看你编写的代码是否按预期工作。为此，运行以下命令：

```
python3 xlsx_demo.py
```

命令执行完成后，你会看到在执行命令的目录中出现了一个名为 sample_workbook.xlsx 的新文件。为验证其是否包含正确的结果，请使用 Microsoft Excel 或 Google Sheets 打开此文件并查看其内容。它应类似于此处所示的内容：

![](img/4840a43efd335abd10b93708225692d6_596_0.png)

图 13.6：使用 xlsxwriter 生成的 Excel 表格

借助 xlsxwriter 模块，你还可以对列应用公式。例如，如果你想添加一行来显示电子表格中人员的平均年龄，可以通过修改 write_data() 方法来实现，如下所示：

```
def write_data(worksheet, data):
    """Write data to the worksheet."""
    for row in range(len(data)):
        for col in range(len(data[row])):
            worksheet.write(row, col, data[row][col])
    worksheet.write(len(data), 0, "Avg. Age")
    # len(data) will give the next index to write to
    avg_formula = "=AVERAGE(B{}:B{})".format(
        1, len(data))
    worksheet.write(len(data), 1, avg_formula)
```

在前面的代码片段中，我们向工作表添加了一个额外的 `write` 调用，并使用了 Excel 提供的 `AVERAGE` 函数来计算工作表中人员的平均年龄。

至此，你现在知道如何使用 Python 生成与 Microsoft Excel 兼容的 XLSX 文件，以及如何导出可供组织中不同团队轻松使用的表格内容。

现在，让我们介绍另一种在全球范围内广泛使用的有趣文件格式：PDF。

## 在 Python 中处理 PDF 文件

**Portable Document Format 或 PDF** 是世界上最常见的文件格式之一。你一定在某个时候遇到过 PDF 文档。这些文档可以包括业务报告、电子书等。

你还记得遇到过带有 **Print page as PDF** 按钮的网站吗？许多政府机构的网站都 readily 提供此选项，允许你直接将网页打印为 PDF。那么问题来了，我们如何为我们的 Web 应用实现这一点？我们应如何添加将某些内容导出为 PDF 的选项？

多年来，庞大的开发者社区为 Python 生态系统贡献了大量有用的包。其中一个包可以帮助我们实现 PDF 文件生成。

## 将网页转换为 PDF

有时，我们可能会遇到想要将网页转换为 PDF 的情况。例如，我们可能想打印网页以将其存储为本地副本。这在尝试打印原生以网页形式显示的证书时也很有用。

为了帮助我们完成此类工作，我们可以利用一个名为 weasyprint 的简单库，该库由 Python 开发者社区维护，允许网页快速轻松地转换为 PDF。因此，让我们看看如何生成网页的 PDF 版本。

## 练习 13.04 – 在 Python 中生成网页的 PDF 版本

在本练习中，我们将使用 Python 生成网站的 PDF 版本。我们将使用一个名为 weasyprint 的社区贡献的 Python 模块来帮助我们生成 PDF。让我们开始吧：

- 1. 为了使即将到来的步骤中的代码正常工作，通过运行以下命令在你的系统上安装 weasyprint 模块：

```
```bash
pip install weasyprint
```
```

### 重要说明

weasyprint 依赖于 cairo 库。如果你尚未安装它，使用 weasyprint 可能会引发错误，并显示 libcairo-2.dll 文件未找到的消息。要解决此错误，请按照 weasyprint 安装链接 https://weasyprint.readthedocs.io/en/stable/install.html#windows 中提到的步骤操作。

- 2. 安装完包后，创建一个名为 pdf_demo.py 的新文件，用于存放 PDF 生成逻辑。在此文件中，编写以下代码：

```
from weasyprint import HTML

def generate_pdf(url, pdf_file):
    """Generate PDF version of the provided URL."""
    print("Generating PDF...")
    HTML(url).write_pdf(pdf_file)
```

现在，让我们尝试理解这段代码的作用。在第一行，我们从 weasyprint 包导入了 HTML 类，该包你在步骤 1 中安装了：

```
from weasyprint import HTML
```

此 HTML 类为我们提供了一种机制，通过该机制，如果我们有其 URL，我们可以读取网站的 HTML 内容。

在下一步中，我们创建了一个名为 generate_pdf() 的新方法，它接受两个参数 – 应用作生成 PDF 的源 URL 的 URL，以及 pdf_file 参数，它接受应写入文档的文件名：

```
def generate_pdf(url, pdf_file):
```

接下来，我们将 URL 传递给前面导入的 HTML 类对象。这导致 URL 被 weasyprint 库解析并读取其 HTML 内容。完成此操作后，我们调用 HTML 类对象的 write_pdf() 方法并提供应写入内容的文件名：

```
HTML(url).write_pdf(pdf_file)
```

- 3. 之后，编写入口点代码，设置应用于演示的 URL（在本练习中，我们将使用 **National Public Radio (NPR)** 网站的文本版本）以及应用于写入 PDF 内容的文件名。设置完成后，代码调用 `generate_pdf()` 方法生成内容：

```
```python
if __name__ == '__main__':
    url = 'http://text.npr.org'
    pdf_file = 'demo_page.pdf'
    generate_pdf(url, pdf_file)
```
```

- 4. 现在，为查看代码的运行情况，运行以下命令：

```
```bash
python3 pdf_demo.py
```
```

命令执行完成后，你将在执行命令的同一目录中获得一个名为 `demo_page.pdf` 的新 PDF 文件。打开文件时，它应类似于此处所示的内容：

![](img/4840a43efd335abd10b93708225692d6_599_0.png)

图 13.7：使用 weasyprint 将网页转换为 PDF

在生成的 PDF 文件中，我们可以看到内容似乎缺少实际网站的格式。这是因为 weasyprint 包读取 HTML 内容但不解析页面的附加 CSS 样式表，因此页面格式丢失了。

weasyprint 也使更改页面格式变得非常容易。这可以通过简单地向 write_pdf() 方法引入 stylesheets 参数来完成。对我们 generate_pdf() 方法的简单修改如下所示：

```
from weasyprint import CSS, HTML

def generate_pdf(url, pdf_file):
    """Generate PDF version of the provided URL."""
    print("Generating PDF...")
    css = CSS(string='body{ font-size: 8px; }')
    HTML(url).write_pdf(pdf_file, stylesheets=[css])
```

现在，执行前面的代码后，我们会看到打印的 PDF 版本中 HTML body 内容中所有文本的字体大小为 8px。

> **重要说明**
> weasyprint 中的 HTML 类也能够接受任何本地文件以及原始 HTML 字符串内容，并使用这些文件生成 PDF。有关更多信息，请访问 weasyprint 文档 https://weasyprint.readthedocs.io。

到目前为止，我们已经了解了如何使用 Python 生成不同类型的二进制文件，这可以帮助我们以结构化方式导出数据，或帮助我们打印页面的 PDF 版本。接下来，我们将学习如何使用 Python 生成数据的图形表示。

## 在 Python 中玩转图表

图表是直观表示特定维度内变化数据的绝佳方式。我们在日常生活中经常遇到图表，无论是每周的天气图表、股市走势，还是学生成绩单。

同样，图表在我们处理 Web 应用时也非常有用。对于 Bookr，我们可以使用图表作为视觉媒介来展示用户每周阅读了多少本书。或者，我们可以根据特定时间阅读给定书籍的读者数量来展示书籍随时间的受欢迎程度。现在，让我们看看如何使用 Python 生成绘图并让它们显示在我们的网页上。





## 使用 plotly 生成图表

在尝试可视化应用程序所维护数据中的模式时，图表会非常有用。有许多 Python 库可以支持开发者生成静态或交互式图表。

在本书中，我们将使用 plotly，这是一个社区支持的 Python 库，可以生成图表并将其渲染到网页上。plotly 特别吸引我们的一点是它易于与 Django 集成。

要在你的系统上安装它，可以运行以下命令：

```
pip install plotly
```

现在你已经完成了安装，让我们看看如何使用 plotly 生成一个图表可视化。

## 设置图形对象

在开始生成图表之前，我们需要初始化一个 plotly Figure 对象，它本质上充当图表的容器。plotly Figure 对象非常容易初始化；可以通过以下代码片段完成：

```
from plotly.graph_objs import graphs
figure = graphs.Figure()
```

plotly 库的 `graph_objs` 模块中的 `Figure()` 构造函数返回一个 Figure 图形容器的实例，图表可以在其中生成。一旦 Figure 对象就绪，我们必须生成一个绘图。

## 生成绘图

绘图是数据集的可视化表示。这个绘图可以是散点图、折线图、图表等等。例如，要生成一个散点图，可以使用以下代码片段：

```
scatter_plot = graphs.Scatter(x_axis_values, y_axis_values)
```

`Scatter` 构造函数接收 X 轴和 Y 轴的值，并返回一个可用于构建散点图的对象。一旦生成了 `scatter_plot` 对象，下一步就是将此绘图添加到我们的 Figure 中。可以按如下方式完成：

```
figure.add_trace(scatter_plot)
```

`add_trace()` 方法负责将绘图对象添加到图形中，并在图形内部生成其可视化。

## 在网页上渲染绘图

一旦绘图被添加到图形中，就可以通过调用 plotly 库的 `offline` 绘图模块中的 `plot()` 方法将其渲染到网页上。如下面的代码片段所示：

```
from plotly.offline import plot
visualization_html = plot(figure, output_type='div')
```

`plot()` 方法接受两个主要参数：第一个是需要渲染的图形，第二个是容器 HTML 标签，图形 HTML 将在此容器内部生成。`plot` 方法返回完全集成的 HTML，可以嵌入到任何网页中，或作为模板的一部分来渲染图表。

现在，了解了图表绘图的工作原理后，让我们尝试一个动手练习，为我们示例数据集生成一个图表。

## 练习 13.05 – 在 Python 中生成图表

在本练习中，我们将使用 Python 生成一个图表绘图。它将是一个表示二维数据的散点图：

- 1. 在本练习中，我们将使用 `plotly` 库。要使用此库，我们需要通过运行以下命令将其安装到系统上：
  
      ```bash
      pip install plotly
      ```

- 2. 现在库已安装，创建一个名为 `scatter_plot_demo.py` 的新文件，并向其中添加以下导入语句：
  
      ```python
      from plotly.offline import plot
      import plotly.graph_objs as graphs
      ```

- 3. 导入完成后，创建一个名为 `generate_scatter_plot()` 的方法，该方法接受两个参数——X 轴的值和 Y 轴的值：
  
      ```python
      def generate_scatter_plot(x_axis, y_axis):
          """Generate a scatter plot for the provided x and
             y-axis values."""
      ```

- 4. 在此方法内部，首先创建一个对象作为图表的容器：
  
      ```python
      figure = graphs.Figure()
      ```

5. 图表容器设置完成后，使用 X 轴和 Y 轴的值创建一个新的 Scatter 对象，并将其添加到图表的 Figure 容器中：

```
scatter = graphs.Scatter(x=x_axis, y=y_axis)
figure.add_trace(scatter)
```

6. 散点图就绪并已添加到图形后，最后一步是生成 HTML，该 HTML 可用于在网页内渲染此绘图。为此，调用 `plot()` 方法并将图形容器对象传递给它。然后，在 HTML div 标签内渲染 HTML：

```
return plot(figure, output_type='div')
```

完整的 `generate_scatter_plot()` 方法现在应如下所示：

```
def generate_scatter_plot(x_axis, y_axis):
    figure = graphs.Figure()
    scatter = graphs.Scatter(x=x_axis, y=y_axis)
    figure.add_trace(scatter)
    return plot(figure, output_type='div')
```

7. 一旦生成了绘图的 HTML，就需要将其渲染到某个地方。为此，创建一个名为 `generate_html()` 的新方法，该方法将绘图 HTML 作为参数，并渲染一个包含该绘图的 HTML 文件：

```
def generate_html(plot_html):
    """Generate an HTML page for the provided plot."""
    html_content = "<html><head><title>Plot Demo</title></head><body>{}</body></html>"
    .format(plot_html)
    try:
        with open('plot_demo.html', 'w') as plot_file:
            plot_file.write(html_content)
    except (IOError, OSError) as file_io_error:
        print("Unable to generate plot file. Exception: {}".format(file_io_error))
```

8. 方法设置完成后，最后一步是调用它。为此，创建一个脚本入口点，设置 X 轴列表和 Y 轴列表的值，然后调用 `generate_scatter_plot()` 方法。使用该方法返回的值，调用 `generate_html()` 方法，该方法将创建一个包含散点图的 HTML 页面：

```
if __name__ == '__main__':
    x = [1,2,3,4,5]
    y = [3,8,7,9,2]
    plot_html = generate_scatter_plot(x, y)
    generate_html(plot_html)
```

9. 代码就绪后，运行该文件并查看生成的输出。要运行代码，请执行以下命令：

```
python3 scatter_plot_demo.py
```

执行完成后，将在脚本执行的同一目录中生成一个新的 `plot_demo.html` 文件。打开该文件后，你应该会看到以下内容：

![](img/4840a43efd335abd10b93708225692d6_604_0.png)

![](img/4840a43efd335abd10b93708225692d6_604_1.png)

图 13.8：使用 plotly 在浏览器中生成的图表

至此，我们已经生成了第一个散点图，其中不同的点通过一条线连接起来。

在本练习中，我们使用 plotly 库生成了一个可以在浏览器中渲染的图表，以便读者可视化数据。

现在，你了解了如何在 Python 中处理图表，以及如何从它们生成 HTML 页面。

但作为 Web 开发者，你如何在 Django 中使用这些图表呢？让我们来了解一下。

## 将 plotly 与 Django 集成

plotly 生成的图表非常容易嵌入到 Django 模板中。由于 `plot()` 方法返回完全包含的 HTML，可用于渲染图表，我们可以将返回的 HTML 作为 Django 中的模板变量并按原样传递。然后，Django 模板引擎将在最终模板显示在浏览器中之前，将该生成的 HTML 添加到最终模板中。

以下是一些示例代码：

```
def user_profile(request):
    username = request.user.get_username()
    scatter_plot_html = scatter_plot_books_read(username)
    return render(request, 'user_profile.html',
                  context={'plt_div': scatter_plot_html})
```

前面的代码将使模板中使用的 `{{ plt_div }}` 内容被存储在 `scatter_plot_demo` 变量中的 HTML 替换，最终模板将渲染每周阅读书籍数量的散点图。

现在，让我们深入探讨如何处理向 Django 添加可视化，以及通过绘图和图表增强应用程序体验。

## 将可视化与 Django 集成

在前几节中，你学习了如何以不同格式读写数据，以满足用户的不同需求。但是我们如何将所学内容与 Django 集成呢？

例如，在 Bookr 中，我们可能希望允许用户导出他们阅读过的书籍列表，或者可视化他们一年中的阅读活动。这该如何实现？下一个练习将聚焦于这一方面。你将学习目前所见的组件如何集成到 Django Web 应用程序中。

## 练习 13.06 – 在用户个人资料页面上可视化用户的阅读历史

在本练习中，我们将修改用户的个人资料页面，以便用户在访问 Bookr 上的个人资料页面时，能够可视化其书籍阅读历史。

让我们看看如何实现：

- 1. 要开始集成可视化用户阅读历史的功能，你需要安装 plotly 库。为此，请在终端中运行以下命令：
  
  ```
  pip install plotly
  ```





2. 安装完库后，下一步是编写代码来获取用户总共阅读的书籍数量以及用户按月阅读的书籍数量。为此，在 `bookr` 应用目录下创建一个名为 `utils.py` 的新文件，并添加所需的导入，这些导入将用于从 `reviews` 应用的 `Review` 模型中获取用户的图书阅读历史：

```python
import datetime

from django.db.models import Count
from reviews.models import Review
```

3. 接下来，创建一个名为 `get_books_read_by_month()` 的新工具方法，该方法接收需要获取阅读历史的用户的用户名。

4. 在此方法内部，我们将查询 `Review` 模型并返回用户按月阅读的书籍字典：

```python
def get_books_read_by_month(username):
    """Get the books read by the user on per month basis.

    :param: str The username for which the books needs to be returned

    :return: dict of month wise books read
    """
    current_year = datetime.datetime.now().year

    books = Review.objects.filter(
        creator__username__contains=username,
        date_created__year=current_year
    ).values('date_created__month').annotate(book_count=Count('book__title'))
    return books
```

现在，让我们检查以下查询，该查询负责获取今年每月阅读书籍的结果：

```python
Review.objects.filter(
    creator__username__contains=username,
    date_created__year=current_year
).values('date_created__month').annotate(book_count=Count('book__title'))
```

```python
# Set the value for books read per month on Y-Axis
for num_books_read in books_read_by_month:
    list_index = num_books_read['date_created__month'] - 1
    books_read[list_index] = num_books_read['book_count']

# Generate a scatter plot HTML
figure = graphs.Figure()
scatter = graphs.Scatter(x=months, y=books_read)
figure.add_trace(scatter)
figure.update_layout(xaxis_title="Month",
                     yaxis_title="No. of books read")
plot_html = plot(figure, output_type='div')
# Add to template
return render(request, 'profile.html',
              {'user': user, 'permissions': permissions,
               'books_read_plot': plot_html})
```

在此方法中，你做了几件事。首先，你调用了 `get_books_read_by_month()` 方法并提供了当前登录用户的用户名。此方法返回给定用户在当年按月阅读的书籍列表：

```python
books_read_by_month = get_books_read_by_month(user.username)
```

你接下来做的就是使用一些默认值预先初始化图表的 X 轴和 Y 轴。对于此可视化，使用 X 轴显示月份，Y 轴显示阅读书籍的数量。

现在，由于你已经知道一年只有 12 个月，因此使用 1 到 12 之间的值预先初始化 X 轴：

```python
months = [i+1 for i in range(12)]
```

对于已阅读的书籍，将 Y 轴初始化为全部 12 个索引都设为 0，如下所示：

```python
books_read = [0 for _ in range(12)]
```

现在预初始化完成后，填充每月阅读书籍的实际值。为此，遍历对 `get_books_read_by_month(user.username)` 的调用所得到的结果列表，并从中提取月份和当月的书籍数量。

一旦提取了书籍数量和月份，下一步是将 `book_count` 值分配给 `books_read` 列表的月份索引：

```python
for num_books_read in books_read_by_month:
    list_index = num_books_read['date_created__month'] - 1
    books_read[list_index] = num_books_read['book_count']
```

现在轴的值已设置，使用 plotly 库生成散点图：

```python
figure = graphs.Figure()
scatter = graphs.Scatter(x=months, y=books_read)
figure.add_trace(scatter)
figure.update_layout(xaxis_title="Month",
                     yaxis_title="No. of books read")
plot_html = plot(figure, output_type='div')
```

生成图表的 HTML 后，使用 `render()` 方法将其传递给模板，以便在个人资料页面上可视化：

```python
return render(request, 'profile.html',
              {'user': user, 'permissions': permissions,
               'books_read_plot': plot_html})
```

7. 在创建了视图函数后，下一步是修改模板以渲染此图表。为此，打开 `templates` 目录下的 `profile.html` 文件，并在最后一个 `{% endblock %}` 语句之前添加以下突出显示的代码到文件中：

```html
{% extends "base.html" %}

{% block title %}Bookr{% endblock %}

{% block heading %}Profile{% endblock %}

{% block content %}
    <ul>
        <li>Username: {{ user.username }} </li>
        <li>Name: {{ user.first_name }} {{ user.last_name }}</li>
        <li>Date Joined: {{ user.date_joined }} </li>
        <li>Email: {{ user.email }}</li>
        <li>Last Login: {{ user.last_login }}</li>
        <li>Groups: {{ groups }}{% if not groups %}None{% endif %} </li>
    </ul>
    {% autoescape off %}
    {{ books_read_plot }}
    {% endautoescape %}

{% endblock %}
```

此代码片段将 `books_read_plot` 变量添加到视图函数中传递，以便在我们的 HTML 模板中使用。另请注意，此变量的 `autoescape` 已设置为关闭。这是因为该变量包含由 plotly 库生成的 HTML。如果允许 Django 转义 HTML，你将在个人资料页面上只看到原始 HTML 而不是图表可视化。

至此，你已成功将图表集成到应用程序中。

8. 要尝试可视化，运行以下命令，然后通过访问 http://localhost:8080/accounts/profile 导航到你的用户个人资料

```bash
python manage.py runserver localhost:8080
```

你应该会看到一个类似于此处所示的页面：

![](img/4840a43efd335abd10b93708225692d6_610_0.png)

图 13.9：用户图书阅读历史散点图

在此练习中，你学习了如何将绘图库与 Django 集成以可视化用户的阅读历史。同样，Django 允许你将任何通用 Python 代码集成到 Web 应用程序中，唯一的限制是集成产生的<|message|> 数据应转换为有效的 HTTP 响应，该响应可以由任何标准 HTTP 兼容工具处理，例如 Web 浏览器或命令行工具，例如 curl。

## 活动 13.01 – 将用户的已读书籍导出为 XLSX 文件

在此活动中，你将在 Bookr 中实现一个新的 API 端点，允许你的用户导出并下载他们已阅读的书籍列表作为 XLSX 文件：

1. 安装 `XlsxWriter` 库。
2. 在 `bookr` 应用下创建的 `utils.py` 文件中，创建一个新函数来帮助获取用户已阅读的书籍列表。
3. 在 `bookr` 目录下的 `views.py` 文件中，创建一个新的视图函数，允许用户以 XLSX 格式下载其阅读历史。
4. 要在视图函数中创建 XLSX 文件，首先，创建一个基于 BytesIO 的内存文件，可用于存储来自 `XlsxWriter` 库的数据。
5. 使用临时文件对象的 `getvalue()` 方法读取存储在内存文件中的数据。
6. 最后，使用 `'application/vnd.ms-excel'` 内容类型标头创建一个新的 `HttpResponse` 实例，然后将步骤 5 中获得的数据写入响应对象。
7. 准备好响应对象后，从视图函数返回响应对象。
8. 视图函数准备好后，将其映射到用户可以访问以下载其图书阅读历史的 URL 端点。

映射 URL 端点后，启动应用程序并使用你的用户帐户登录。一旦完成，访问你刚刚创建的 URL 端点，如果访问 URL 端点时浏览器开始下载 Excel 文件，则你已成功完成活动。

> **重要说明**
> 此活动的解决方案可以在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions 上找到。




## 总结

在本章中，我们了解了如何处理二进制文件，以及 Python 标准库（预装了必要工具）如何让我们处理诸如 CSV 之类的常用文件格式。随后，我们继续学习了如何使用 Python 的 csv 模块来读写 CSV 文件。之后，我们使用了 XlsxWriter 包，它允许我们直接在 Python 环境中生成与 Microsoft Excel 兼容的文件，而无需担心文件的内部格式。

本章的后半部分专门介绍了如何使用 weasyprint 库为 HTML 页面生成 PDF 版本。当我们希望为用户提供一个简单的选项来打印页面 HTML 版本并加上我们选择的任何 CSS 样式时，这项技能会很有用。本章的最后一节讨论了如何使用 plotly 库在 Python 中生成交互式图表，并将它们渲染为可在浏览器中查看的 HTML 页面。

在下一章中，我们将探讨如何测试前几章中一直在实现的各种组件，以确保代码更改不会破坏我们网站的功能。

## 14
