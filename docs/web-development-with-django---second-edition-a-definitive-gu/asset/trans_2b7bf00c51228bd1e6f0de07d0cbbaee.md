
在前面的代码片段中，我们首先从 `django.http` 模块导入了 `HttpResponse` 类。接下来，我们定义了 `welcome_view` 函数，它可以在 Web 浏览器中显示 “Welcome to Bookr!” 消息。`request` 对象是一个函数参数，承载着 HTTP 请求对象。下一行定义了 `message` 变量，其中包含显示标题的 HTML，随后一行统计数据库中可用的图书数量。

在最后一行，我们返回一个 `HttpResponse` 对象，其字符串与 `message` 变量相关联。当调用 `welcome_view` 视图函数时，它会在 Web 浏览器中显示 “Welcome to Bookr! 2 books and counting”。

3. 现在，创建 URL 映射以调用新创建的视图函数。打开项目 URL 文件 `bookr/urls.py`，并添加 `urlpatterns` 列表，如下所示：

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('reviews.urls'))
]
```

`urlpatterns` 列表中的第一行，即 `path('admin/', admin.site.urls)`，如果 URL 路径中包含 `admin/`（例如 `http://0.0.0.0:8000/admin`），则会路由到 admin URL。

类似地，考虑第二行 `path('', include('reviews.urls'))`。这里提到的路径是空字符串 `''`。如果 URL 在 `http://hostname:port-number/` 之后没有任何特定路径（例如 `http://0.0.0.0:8000/`），则会包含 `review.urls` 中存在的 `urlpatterns`。

`include` 函数是一个快捷方式，允许你组合 URL 配置。在 Django 项目中，通常每个应用保留一个 URL 配置。这里，我们为 `reviews` 应用创建了一个单独的 URL 配置，并将其添加到项目级 URL 配置中。

4. 由于我们还没有 `reviews.urls` URL 模块，因此创建一个名为 `bookr/reviews/urls.py` 的文件，并添加以下代码行：

```
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('', views.welcome_view,
    name='welcome_view'),
]
```

这里，我们再次对 URL 路径使用了空字符串。因此，当调用 `http://0.0.0:8000/` 时，在从 `bookr/urls.py` 路由到 `bookr/reviews/urls.py` 之后，此模式会调用 `welcome_view` 视图函数。

5. 在对这两个文件进行修改之后，我们已经准备好了调用 `welcome_view` 视图所需的 URL 配置。现在，使用 `./manage.py runserver` 启动 Django 服务器，并在 Web 浏览器中输入 `http://0.0.0:8000` 或 `http://127.0.0.1:8000`。你应该能够看到 “Welcome to Bookr!” 消息：

![](img/4840a43efd335abd10b93708225692d6_151_0.png)

图 3.1 – 在主页上显示 “Welcome to Bookr!” 和图书数量

注意
如果没有 URL 匹配，Django 会调用错误处理，例如显示 “404 Page not found” 消息或类似内容。

在本练习中，我们学习了如何编写基本的视图函数并执行相关的 URL 映射。我们创建了一个网页，向用户显示一条简单的消息，并报告当前数据库中有多少本书。

然而，敏锐的读者会注意到，像前面的示例那样将 HTML 代码放在 Python 函数中看起来并不优雅。随着我们的视图越来越大，这变得更加不可持续。因此，在下一节中，我们将把注意力转向 HTML 代码应该所在的位置——模板内部。

## 使用 Django 模板

在练习 3.01 – 实现一个简单的基于函数的视图中，我们看到了如何创建视图、进行 URL 映射并在浏览器中显示消息。但如果你还记得，我们在视图函数本身中硬编码了 “Welcome to Bookr!” HTML 消息，并返回了一个 `HttpResponse` 对象，如下所示：

```
message = f"<html><h1>Welcome to Bookr!</h1> "
"<p>{Book.objects.count()} books and counting!</p></html>"
return HttpResponse(message)
```

在 Python 模块中硬编码 HTML 并不是一个好习惯，因为随着要在网页上渲染的内容量增加，我们需要为其编写的 HTML 代码量也会增加。在 Python 代码中夹杂大量 HTML 代码会使代码难以阅读，并且从长远来看难以维护。

因此，Django 模板为我们提供了一种更好的方式来编写和管理 HTML 模板。Django 的模板不仅适用于静态 HTML 内容，也适用于动态 HTML 模板。

Django 的模板配置在 `settings.py` 文件中的 `TEMPLATES` 变量中完成。默认配置如下所示：

```
TEMPLATES = [
    {
        'BACKEND':
            'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

让我们逐一了解前面代码片段中的每个关键字：

- `'BACKEND': 'django.template.backends.django.DjangoTemplates'`：这指的是要使用的模板引擎。模板引擎是 Django 用于处理 HTML 模板的 API。Django 内置了 Jinja2 和 DjangoTemplates 引擎。默认配置是 DjangoTemplates 引擎和 Django 模板语言。但是，如果需要，可以更改为其他引擎，例如 Jinja2 或任何其他第三方模板引擎。不过，对于我们的 Bookr 应用程序，我们将保持此配置不变。
- `'DIRS': []`：这指的是 Django 按给定顺序搜索模板的目录列表。
- `'APP_DIRS': True`：这告诉 Django 模板引擎是否应该在 `settings.py` 文件的 `INSTALLED_APPS` 下定义的已安装应用中查找模板。此选项的默认值是 `True`。
- `'OPTIONS'`：这是一个包含模板引擎特定设置的字典。在此字典中，有一个默认的上下文处理器列表，它帮助 Python 代码与模板交互，以创建和渲染动态 HTML 模板。

当前的默认设置大多适合我们的目的。但是，在下一个练习中，我们将为模板创建一个新目录，并且需要指定此文件夹的位置。例如，如果我们有一个名为 `my_templates` 的目录，我们需要通过将其添加到 `TEMPLATES` 设置中来指定其位置，如下所示：

```
TEMPLATES = [
{
    'BACKEND':
        'django.template.backends.django.DjangoTemplates',
    'DIRS': [os.path.join(BASE_DIR, 'my_templates')],
    'APP_DIRS': True,
    'OPTIONS': {
        'context_processors': [
            'django.template.context_processors.debug',
            'django.template.context_processors.request',
            'django.contrib.auth.context_processors.auth',
            'django.contrib.messages.context_processors.messages',
        ],
    },
},
]
```

`BASE_DIR` 是项目文件夹的目录路径。这在 `settings.py` 文件中定义。`os.path.join()` 方法将项目目录与模板目录连接起来，返回模板目录的完整路径。在接下来的练习中，我们将使用模板向用户显示消息。

## 练习 3.02 – 使用模板显示问候消息

在本练习中，我们将创建第一个 Django 模板，并且就像在之前的练习中所做的那样，我们将使用模板显示 `Welcome to Bookr!` 消息。让我们开始执行这些步骤：

- 1. 在 `bookr` 项目目录中创建一个名为 `templates` 的目录，并在其中创建一个名为 `base.html` 的文件。目录结构应如下图所示：

![](img/4840a43efd335abd10b93708225692d6_154_0.png)

图 3.2 – bookr 的目录结构

> **注意**
> 
> 使用默认配置时（即 `DIRS` 为空列表时），Django 只会搜索应用文件夹的 `templates` 目录中存在的模板（对于图书评论应用，即 `reviews/templates` 文件夹）。由于我们将新的模板目录放在了主项目目录中，因此除非在 `DIRS` 列表中包含该目录，否则 Django 的模板引擎将无法找到它。

- 2. 将该文件夹添加到 `TEMPLATES` 设置中：

```
TEMPLATES = [
    {
        'BACKEND':
            'django.template.backends.django.DjangoTemplates',
        'DIRS': os.path.join(BASE_DIR, 'my_templates'),
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
```
