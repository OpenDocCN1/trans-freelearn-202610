
接下来，我们将了解 Django 如何定位模板文件。

## 在应用目录中查找 HTML 模板

有许多选项可用来告诉 Django 如何查找模板，这些选项可以在 settings.py 的 TEMPLATES 设置中进行配置，但（目前）最简单的做法是在 reviews 目录内创建一个 templates 目录。由于 settings.py 中的 APP_DIRS 为 True（如上一节所见），Django 会查找该目录（以及其他应用的 templates 目录）。然而，要让 Django 知道 reviews 目录是一个应用，我们需要在设置中对其进行配置。我们将在下一个练习中完成这一点。

## 练习 1.05 – 创建 templates 目录和基础模板

在本练习中，你将为 reviews 应用创建一个 templates 目录。然后，你将添加一个 HTML 模板文件，Django 可以将其渲染为 HTTP 响应。

我们在“探索 Django 设置”一节中讨论过 settings.py 及其 INSTALLED_APPS 设置。我们需要将 reviews 应用添加到 INSTALLED_APPS 中，以便 Django 能够找到模板。请按照以下步骤操作：

- 1. 在 PyCharm 中打开 settings.py。更新 INSTALLED_APPS 设置，并在末尾添加 reviews。它应如下所示：

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'reviews'
]
```

图 1.31 – 已将 reviews 应用添加到 settings.py

- 2. 保存并关闭 settings.py。

- 3. 在 PyCharm 的项目浏览器中，右键单击 reviews 目录，然后选择“新建 | 目录”。

图 1.32 – 在 reviews 目录内新建一个目录

4. 输入名称 `templates`，然后按 *Enter/Return* 键创建它：

![](img/4840a43efd335abd10b93708225692d6_67_0.png)

图 1.33 – 将目录命名为 templates

5. 右键单击新创建的 `templates` 目录，然后选择 **新建 | HTML 文件**。

![](img/4840a43efd335abd10b93708225692d6_67_1.png)

图 1.34 – 在 templates 目录中新建一个 HTML 文件

6. 在出现的窗口中，输入名称 `base.html`，保持选中 **HTML 5 文件**，然后按 *Enter/Return* 键创建文件：

![](img/4840a43efd335abd10b93708225692d6_67_2.png)

图 1.35 – 新建 HTML 文件窗口

7. PyCharm 创建文件后，也会自动打开该文件。其内容如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

8. 在 <body> 和 </body> 标签之间，添加一条简短消息，以验证模板正在被渲染：

```
<body>
    Hello from a template!
</body>
```

以下截图展示了你的完整代码应呈现的样子：

![](img/4840a43efd335abd10b93708225692d6_68_0.png)

图 1.36 – 带有一些示例文本的 base.html 模板

在本练习中，我们为 reviews 应用创建了一个 templates 目录，并添加了一个 HTML 模板。一旦我们在视图上实现 render 函数的使用，该 HTML 模板就会被渲染。

## 使用 render 函数渲染模板

现在我们有一个可用的模板，但我们需要更新 index 视图，使其渲染该模板，而不是像当前那样返回 “Hello (name)!” 文本（可参考图 1.30 查看当前效果）。我们将通过使用 render 函数并提供模板名称来实现这一点。render 是一个返回 HttpResponse 的快捷函数。还有其他渲染模板的方法可以提供更多控制，但目前这个函数已足够满足我们的需求。render 至少接受两个参数——第一个始终是传递给视图的 request，第二个是被渲染模板的名称/相对路径。我们还将使用第三个参数调用它，即包含模板中所有可用变量的渲染上下文——有关这方面的更多内容，将在练习 1.07“在模板中使用变量”中介绍。

## 练习 1.06 – 在视图中渲染模板

在本练习中，你将更新 index 视图函数，以渲染你在练习 1.05“创建 templates 目录和基础模板”中创建的 HTML 模板。你将使用 render 函数，该函数会从磁盘加载你的模板，渲染它，并将其发送到浏览器。这将取代你当前从 index 视图函数返回的静态文本：

- 1. 在 PyCharm 中，打开 reviews 目录中的 views.py。
- 2. 我们不再手动创建 HttpResponse，因此删除导入 HttpResponse 的那一行：

```
from django.http import HttpResponse
```

- 3. 将其替换为从 django.shortcuts 导入 render 函数的语句：

```
from django.shortcuts import render
```

- 4. 更新 index 函数，使其不再返回 HttpResponse，而是返回对 render 的调用，并传入 request 和模板名称：

```
def index(request):
    return render(request, "base.html")
```

以下截图展示了完整的 views.py 文件：

![](img/4840a43efd335abd10b93708225692d6_70_0.png)

图 1.37 – 完成了模板渲染的 view.py

5. 如果开发服务器尚未运行，请启动它。然后，打开你的 Web 浏览器并刷新 http://127.0.0.1:8000。你应该会看到渲染出的“Hello from a template!”消息，如图 1.38 所示：

![](img/4840a43efd335abd10b93708225692d6_70_1.png)

图 1.38 – 你渲染的第一个 HTML 模板

## 在模板中渲染变量

模板并非仅仅是静态 HTML。大多数情况下，它们会包含在渲染过程中被插值的变量。这些变量通过上下文（context）从视图传递给模板——上下文是一个字典（或类似字典的对象），包含模板可以使用的所有变量的名称。我们再次以 Bookr 为例。如果模板中没有变量，那么每显示一本书，你就需要一个不同的 HTML 文件。相反，我们在模板中使用诸如 book_name 这样的变量，然后视图为模板提供一个 book_name 变量，其值设为其加载的书籍模型的标题。当显示另一本书时，HTML 无需更改；视图只是向它传递一本不同的书。你可以看到模型、视图和模板现在是如何协同工作的。

与 PHP 等其他一些语言不同，变量必须显式传递给模板，视图中的变量不会自动对模板可用。这既是出于安全原因，也是为了避免意外污染模板的命名空间（我们不希望模板中出现任何意外的变量）。

在模板内部，变量用双花括号 – {{ }} 表示。虽然这并非严格的标准，但这种风格相当常见，并用于其他模板工具，如 Vue.js 和 Mustache。Symfony（一个 PHP 框架）在其 Twig 模板语言中也使用了双花括号，因此你可能在那里见过类似的用法。

要在模板中渲染变量，只需用花括号将其包裹起来 – {{ book_name }}。Django 会自动对输出中的 HTML 进行转义，因此你可以在变量中包含特殊字符（如 < 或 >），而不必担心它会破坏你的输出。如果某个变量未传递给模板，Django 只会在该位置渲染为空，而不会抛出异常。

还有更多使用过滤器以不同方式渲染变量的方法，但这些将在第 3 章“URL 路由器、视图和模板”中介绍。

## 练习 1.07 – 在模板中使用变量

我们将在 base.html 文件中放入一个简单变量，以演示 Django 的变量插值是如何工作的：

- 1. 在 PyCharm 中，打开 base.html 文件。
- 2. 更新 <body> 元素，使其包含一个渲染 name 变量的位置：

```
<body>
Hello, {{ name }}!
</body>
```

3. 回到你的 Web 浏览器并刷新（你应该仍在 http://127.0.0.1:8000）。你会看到页面现在显示 “Hello, !”。这是因为我们尚未在渲染上下文中设置 name 变量。

![](img/4840a43efd335abd10b93708225692d6_72_0.png)

图 1.39 – 由于未设置上下文，模板中没有渲染出任何值

4. 打开 views.py，在 index 函数中添加一个名为 name 的变量，并将其值设置为 "world"：

```
def index(request):
    name = "world"
    return render(request, "base.html")
```

5. 再次刷新浏览器。请注意，没有任何变化——我们想要渲染的任何内容都必须作为上下文显式传递给 render 函数。这是在渲染时可供使用的变量字典。

6. 将上下文字典作为第三个参数传递给 render 函数。将你的 render 代码行改为如下：

```
return render(request, "base.html", {"name": name})
```

