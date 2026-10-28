

在这里，在新的模板文件中，我们创建了一个无序列表，用于存放用户已读的书籍列表。接下来，使用 for 模板标签，我们遍历由自定义模板函数提供的 book_list 中的值：

```
{% for book in book_list %}
```

此迭代会根据以下内容创建多个列表项：

```
<li>{{ book }}</li>
```

列表项会使用来自 book_list 的内容生成并传递给模板。for 标签执行的次数与 book_list 中存在的项目数量相同。

3. 在为 book_list 标签定义了模板后，修改现有的问候模板以使其在模板内部可用，并使用它来显示用户已读的书籍列表。为此，修改 filter_demo/templates 目录下的 simple_tag_template.html 文件，并将代码更改如下：

```
<html>
<head>
<title>Simple Tag Template Example</title>
</head>
<body>
{% load simple_tag inclusion_tag %}
{% greet_user "Hey" username %}
<br />
<span class="message">You have read the following
books till date</span>
{% book_list books %}
</body>
</html>
```

在此片段中，你做的第一件事是通过编写以下内容加载 inclusion_tag 模块：

```
{% load simple_tag inclusion_tag %}
```

加载标签后，你现在可以在模板中的任何位置使用它。为此，你以以下格式添加了 book_list 标签：

```
{% book_list books %}
```

此标签接受一个参数，即书籍的字典，其中键为书名，键的值为该书的作者。

4. 在模板修改完成后，最后一步涉及将所需数据传递给模板。为此，修改 filter_demo 目录中的 views.py 并将 greeting 视图函数更改如下：

```
def greeting_view(request):
    books = {
"The night rider": "Ben Author",
"The Justice": "Don Abeman"
}
return render(request, 'simple_tag_template.html',
{'username': 'jdoe', 'books': books})
```

在这里，我们修改了 greeting_view 函数以添加书籍及其作者的字典，然后将其传递给 simple_tag_template 上下文。

完成更改后，文件应类似于托管在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter11/Exercise11.03 的文件。

5. 在实施了上述更改后，现在是时候渲染修改后的模板了。为此，通过运行以下命令重新启动 Django 应用程序服务器：

```
python manage.py runserver localhost:8080
```

6. 导航到 http://localhost:8080/greet，它现在应渲染一个类似于以下屏幕截图的页面：

![](img/4840a43efd335abd10b93708225692d6_536_0.png)

图 11.3 – 用户访问问候端点时已读书籍列表

该页面显示了用户访问问候端点时已读的书籍列表。你在页面上看到的列表是使用 inclusion 标签渲染的。用于列出这些书籍的模板是先单独创建的，然后使用 inclusion 标签将其添加到页面中。

至此，我们现在拥有了可以构建高度复杂的模板过滤器或自定义标签的基础，这些标签在我们想要从事的项目开发中会很有帮助。

## 现在，让我们来看看 Django 视图，并深入到 Django 提供的基于类的视图这一新领域，以帮助我们利用面向对象编程的力量，从而实现视图渲染的代码重用。

# Django views

回顾一下，Django 中的视图是一段 Python 代码，它允许接收请求，基于请求执行操作，然后向用户返回响应，从而构成了我们 Django 应用程序的重要组成部分。

在 Django 内部，我们有两种不同的构建视图的方法，其中一种我们在前面的示例中已经看到过，称为基于函数的视图，而另一种我们即将介绍的称为基于类的视图：

- **基于函数的视图 (FBVs)**：Django 中的 FBVs 只不过是普通的 Python 函数，它们应该以 `HTTPRequest` 类型对象作为第一个位置参数，并返回对应于视图在请求被处理后想要执行的操作的 `HTTPResponse` 类型对象。在前面的练习中，`index()` 和 `greeting_view()` 就是 FBVs 的示例。
- **基于类的视图 (CBVs)**：CBVs 是严格遵循 Python 面向对象原则的视图，允许将视图调用映射为基于类的表示。这些视图具有专门的性质，给定类型的 CBV 执行特定操作。CBV 提供的优势包括视图易于扩展和代码重用，这在 FBVs 中可能是一项复杂的任务。

现在，基本定义已经明确，并且我们已经掌握了 FBVs 的知识，让我们来看看 CBVs，看看它们为我们准备了什么。

# Class-based views

Django 提供了开发人员为其应用程序编写视图的不同方式。一种方式是将 Python 函数映射为视图函数以创建 FBVs。创建视图的另一种方式是使用 Python 对象实例（基于 Python 类）。这些被称为 CBVs。出现的一个重要问题是，当我们已经可以使用 FBV 方法创建视图时，为什么还需要 CBV？

在创建 FBVs 时，想法是，有时我们可能会一遍又一遍地复制相同的逻辑，例如处理某些字段或处理某些请求类型的逻辑。虽然完全可以创建在逻辑上分离的函数来处理特定逻辑，但随着应用程序复杂性的增加，管理任务变得困难。

这就是 CBVs 派上用场的地方，因为它们抽象掉了我们需要编写的常见重复代码的实现，以处理某些任务，例如模板的渲染，同时还通过使用继承和混入使代码片段易于重用。例如，以下代码片段显示了 CBV 的实现：

```
```python
from django.http import HttpResponse
from django.views import View

class IndexView(View):

    def get(self, request):
        return HttpResponse("Hey there!")
```
```

在前面的示例中，我们通过继承 Django 提供的内置视图类构建了一个简单的 CBV。

使用这些 CBVs 也非常容易。例如，假设我们想将 `IndexView` 映射到应用程序中的 URL 端点。在这种情况下，我们只需在应用程序的 `urls.py` 文件中的 `urlpatterns` 列表中添加以下行即可：

```
```python
urlpatterns = [
    path('my_path', IndexView.as_view(), name='index_view')
]
```
```

在这里，正如我们所观察到的，我们使用了我们创建的 CBV 的 `as_view()` 方法。每个 CBV 都实现了 `as_view()` 方法，该方法允许视图类通过从视图类返回视图控制器的实例来映射到 URL 端点。

Django 提供了一些内置的 CBV，它们提供了大量常见任务的实现，例如如何渲染模板或如何处理特定请求。内置的 CBV 有助于避免在处理基本功能时从头开始重写代码，从而实现代码的重用。其中一些内置视图包括以下内容：

- **View**：这是 Django 中所有 CBV 的基类，允许使用所有提供的功能编写自定义 CBV，并且可以覆盖。用户可以为不同的 HTTP 请求方法实现自己的定义，例如 GET、POST、PUT 和 DELETE，并且视图将根据收到的请求类型自动将调用委托给负责处理请求的方法。
- **TemplateView**：这是一个可用于根据调用 URL 中提供的模板数据参数渲染模板的视图。这使开发人员能够轻松渲染模板，而无需编写与渲染处理相关的任何逻辑。
- **RedirectView**：这是一个视图，可以根据用户发出的请求自动将用户重定向到正确的资源。

