

5. 添加以下代码来实现 explode 过滤器：

```
@register.filter
def explode(value, separator):
    return value.split(separator)
```

explode 过滤器接收两个参数；一个是使用该过滤器的 value，另一个是 separator，从模板传递给过滤器。过滤器将使用此分隔符将字符串转换为列表。

6. 自定义过滤器准备好后，创建一个可在此过滤器上应用的模板。为此，首先在 filter_demo 目录下创建一个名为 templates 的新文件夹，然后在其中创建一个名为 index.html 的新文件，其内容如下：

```
<html>
<head>
<title>Custom Filter Example</title>
<body>
{% load explode_filter %}

{{ names|explode:"," }}
</body>
</html>
```

在第一行，Django 的模板引擎从 explode_filter 模块加载自定义过滤器，以便在模板内部使用。为此，Django 将在 templatetags 目录下查找 explode_filter 模块，如果找到，则会加载它以供使用。

在下一行，你将传递给模板的 names 变量传递过去并对其应用 explode 过滤器，同时将 , 符号作为分隔符值传递给过滤器。

7. 现在模板已创建，接下来要创建的是一[content]

5. 添加以下代码来实现 explode 过滤器：

```
@register.filter
def explode(value, separator):
    return value.split(separator)
```

explode 过滤器接收两个参数；一个是使用该过滤器的 value，另一个是 separator，从模板传递给过滤器。过滤器将使用此分隔符将字符串转换为列表。

6. 自定义过滤器准备好后，创建一个可在此过滤器上应用的模板。为此，首先在 filter_demo 目录下创建一个名为 templates 的新文件夹，然后在其中创建一个名为 index.html 的新文件，其内容如下：

```
<html>
<head>
<title>Custom Filter Example</title>
<body>
{% load explode_filter %}

{{ names|explode:"," }}
</body>
</html>
```

在第一行，Django 的模板引擎从 explode_filter 模块加载自定义过滤器，以便在模板内部使用。为此，Django 将在 templatetags 目录下查找 explode_filter 模块，如果找到，则会加载它以供使用。

在下一行，你将传递给模板的 names 变量传递过去并对其应用 explode 过滤器，同时将 , 符号作为分隔符值传递给过滤器。

7. 现在模板已创建，接下来要创建的是一个可以渲染此模板并将 name 变量传递给模板的 Django 视图。为此，打开 views.py 文件并添加以下高亮代码：

```
from django.shortcuts import render

def index(request):
    names = "john,doe,mark,swain"
    return render(request, "index.html", {'names':
        names})
```

前面的代码片段执行了一些基本操作。它首先从 django.shortcuts 模块导入 render 辅助函数，该函数有助于渲染模板。导入完成后，它定义了一个名为 index() 的新视图函数，该函数渲染 index.html。

8. 现在将视图映射到 URL，以便随后可以在浏览器中渲染结果。为此，在 filter_demo 目录内创建一个名为 urls.py 的新文件并添加以下代码：

```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index')
]
```

9. 将 filter_demo 应用程序添加到项目 URL 映射中。为此，打开 bookr 项目目录中的 urls.py 并在 urlpatterns 内部添加以下高亮行：

```
urlpatterns = [
    path('filter_demo/', include('filter_demo.urls')),
    ...
]
```

进行更改后，bookr 项目目录下的 urls.py 文件应类似于 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter11/Exercise11.01/bookr/urls.py 下所示的文件。

10. 最后，在 bookr 项目的 settings.py 的 INSTALLED_APPS 部分下添加该应用程序：

```
INSTALLED_APPS = [
    ...,
    'filter_demo'
]
```

此要求源于 Django 实施的安全指南，该指南要求实现自定义过滤器/标签的应用程序需要添加到 INSTALLED_APPS 部分。进行更改后，settings.py 文件应类似于 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter11/Exercise11.01/bookr/settings.py 下的文件。

11. 要查看自定义过滤器是否有效，运行以下命令：

```
python manage.py runserver localhost:8000
```

现在，导航到浏览器中的以下页面：http://localhost:8000/filter_demo。

12. 此页面应如图 11.1 所示：

![](img/4840a43efd335abd10b93708225692d6_525_0.png)

图 11.1 – 使用 explode 过滤器显示的索引页面

至此，我们看到了如何在 Django 中快速创建一个自定义过滤器，然后在模板中使用它。在下一节中，让我们看一下另一种类型的过滤器，即字符串过滤器，它仅对字符串类型的值起作用。

## 字符串过滤器

在练习 11.01 创建自定义模板过滤器 中，我们构建了一个自定义过滤器，它允许我们使用分隔符拆分提供的字符串并从中生成列表。此过滤器可以接受任何类型的变量并根据提供的分隔符将其拆分为值列表。但如果我们希望限制过滤器仅对字符串有效，而不对其他类型的值，例如整数有效，该怎么办？

我们可以使用 Django 模板库提供的 stringfilter 装饰器来开发仅对字符串起作用的过滤器。当使用 stringfilter 装饰器将 Python 方法注册为 Django 中的过滤器时，框架会确保在过滤器执行之前将传递给过滤器的值转换为字符串。这减少了当非字符串值传递给我们的过滤器时可能出现的问题。

实现 **字符串过滤器** 的步骤与我们为构建自定义过滤器所遵循的步骤类似，只是有些细微的改动。

还记得我们在 *设置存储模板过滤器的目录* 部分中创建的 `custom_filter.py` 文件吗？让我们现在在其中添加一个新的 Python 函数，它将充当我们的字符串过滤器。

在我们实现字符串过滤器之前，我们首先需要导入 `stringfilter` 装饰器，该装饰器将自定义过滤器函数标记为字符串过滤器。你可以通过在 `custom_filters.py` 文件中添加以下导入语句来添加此装饰器：

```
from django.template.defaultfilters import stringfilter
```

现在，要实现我们的自定义字符串过滤器，可以使用以下语法：

```
```python
@register.filter
@stringfilter
def generic_string_filter(value, arg):
    # Logic for string filter implementation
```
```

通过这种方法，我们可以构建任意数量的字符串过滤器，并像使用任何其他过滤器一样使用它们。

## 模板标签

模板标签是 Django 模板引擎的一个强大功能。它们允许开发人员通过评估某些条件来生成 HTML，从而构建强大的模板，并避免重复编写常见代码。

一个可能使用模板标签的示例是网站导航栏中的注册/登录选项。在这种情况下，我们可以使用模板标签来评估当前页面上的访问者是否已登录。基于此，我们可以渲染个人资料横幅或注册/登录横幅。

在开发模板时，标签也很常见。例如，考虑以下我们上节在模板中导入自定义过滤器时使用的代码行：

```
{% load explode_filter %}
```

这使用了一个名为 `load` 的模板标签，负责将 `explode` 过滤器加载到模板中。与过滤器相比，模板标签功能更强大。虽然过滤器只能访问其操作的值，但模板标签可以访问整个模板的上下文，因此它们可用于在模板内部构建大量复杂功能。

让我们现在来看一下 Django 支持的不同类型的模板标签以及如何构建我们自己的自定义模板标签。

## 模板标签的类型

Django 主要支持两种类型的模板标签：

- **简单标签**：这些标签对提供的变量数据（以及传递给它们的任何附加变量）进行操作，并在调用它们的同一模板中渲染。例如，一个这样的用例可以包括根据用户的用户名向用户渲染自定义欢迎消息，或根据用户名显示用户的最后登录时间。
- **包含标签**：这些标签接收提供的变量数据并通过渲染另一个模板来生成输出。例如，标签可以接收对象列表并对其进行迭代以生成 HTML 列表。

在下一节中，我们将了解如何创建这些不同类型的标签并在我们的应用程序中使用它们。

## 简单标签

简单标签为开发人员提供了一种构建模板标签的方式，该标签接收模板中的一个或多个变量，对其进行处理并返回响应。从模板标签返回的响应会替换 HTML 模板中提供的模板标签定义。这些类型的标签可用于构建多种有用的功能，例如日期解析或显示我们想要向用户展示的任何活动警报。

简单标签可以使用模板库提供的 `simple_tag` 装饰器轻松创建，通过装饰应充当模板标签的 Python 方法。现在，让我们看看如何使用 Django 的模板库实现自定义简单标签。

## 创建简单模板标签

创建简单模板标签遵循我们在 *自定义模板过滤器* 部分中讨论的约定，只是有些细微差别。首先，让我们回顾一下如何为在 Django 模板中使用创建模板标签。

## 设置目录

就像自定义过滤器一样，自定义模板标签也需要创建在同一个 `templatetags` 目录内，以便 Django 的模板引擎可以发现它们。该目录可以使用 PyCharm GUI 直接创建，也可以通过在我们想要创建自定义标签的应用程序目录内运行以下命令来创建：

```
mkdir templatetags
```

