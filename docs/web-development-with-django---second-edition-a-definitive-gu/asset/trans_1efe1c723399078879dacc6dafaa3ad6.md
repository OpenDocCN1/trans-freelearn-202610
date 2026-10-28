

# 练习 8.02 – 模板设置和使用模板中的 MEDIA_URL

在本练习中，你将继续使用 `media_project` 并配置 Django 以自动将 `MEDIA_URL` 设置添加到每个模板。你通过将 `django.template.context_processors.media` 添加到 `TEMPLATES` 上下文处理器设置来实现这一点。然后你将添加一个使用此新变量的模板以及一个用于渲染它的示例视图。你将在本章的练习中不断修改视图和模板：

+   1. 在 PyCharm 中打开 `settings.py`。首先，你需要将 `media_example` 添加到 `INSTALLED_APPS` 设置中，因为在项目设置时并未完成：

```
```python
INSTALLED_APPS = [
    # other apps truncated for brevity
    "media_example",
]
```
```

+   2. 文件大约 halfway 下方，你会找到 `TEMPLATES` 设置，它是一个字典。其内部是 `OPTIONS` 项（另一个字典）。`OPTIONS` 内部是 `context_processors` 设置。

将以下内容添加到此列表的末尾：

```
```python
"django.template.context_processors.media"
```
```

完整的列表应如下所示：

```
```python
TEMPLATES = [
    {
        "BACKEND":
            "django.template.backends.django
            .DjangoTemplates",
        "DIRS": [],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors
                .debug",
                "django.template.context_processors
                .request",
                "django.contrib.auth.context_processors
                .auth",
                "django.contrib.messages
                .context_processors.messages",
                "django.template.context_processors
                .media",
            ],
```
```
```
},
},
]
```

完整的文件应如下所示：https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.02/media_project/media_project/settings.py。

+   3. 打开 `media_example` 应用的 `views.py` 文件并创建一个名为 `media_example` 的新视图。目前，它只需渲染一个名为 `media-example.html` 的模板（你将在步骤 5 中创建）。视图函数的完整代码如下：

```
```python
def media_example(request):
    return render(request, "media-example.html")
```
```

保存 `views.py`。它应如下所示：https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.02/media_project/media_example/views.py。

+   4. 你需要一个指向 `media_example` 视图的 URL 映射。打开 `media_project` 包的 `urls.py` 文件。

首先，使用文件中的其他导入导入 `media_example.views`：

```
```python
import media_example.views
```
```

然后在 `urlpatterns` 中添加路径以将 `media-example/` 映射到 `media_example` 视图：

```
```python
path('media-example/',
     media_example.views.media_example)
```
```

你的完整 urlpatterns 应如下所示的代码块：

```
```python
from django.conf.urls.static import static

import media_example.views

urlpatterns = [
    path("admin/", admin.site.urls),
    path("media-example/",
         media_example.views.media_example)
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT)
```
```

你可以保存并关闭文件。

5. 在 media_example 应用目录内创建 templates 目录。然后，在 media_project 项目 templates 目录内创建一个新的 HTML 文件。选择 HTML 5 文件并将文件命名为 media-example.html：

![](img/4840a43efd335abd10b93708225692d6_397_0.png)

图 8.4：创建 media-example.html

6. media-example.html 文件应自动打开。你只需在文件内添加一个指向你在练习 8.01 – 配置媒体存储和服务中创建的 test.txt 文件的链接。在 <body> 元素内，添加突出显示的代码：

```
<body>
    <a href="{{ MEDIA_URL }}test.txt">Test Text File</a>
</body>
```

注意 MEDIA_URL 和文件名之间没有 / ——这是因为我们在 settings.py 中定义它时已经添加了尾部斜杠。你可以保存文件。完整的文件将如下所示：https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.02/media_project/media_example/templates/media-example.html。

+   7. 如果尚未运行，请启动 Django 开发服务器，然后访问 `http://127.0.0.1:8000/media-example/`。你应该会看到类似 *图 8.5* 的简单页面：

![](img/4840a43efd335abd10b93708225692d6_398_0.png)

如果点击链接，你将被带到 `test.txt` 显示页面并看到你在 *练习 8.01 – 配置媒体存储和服务（图 8.5）* 中创建的 *Hello, world!* 文本。这意味着你已正确配置了 Django `context_processors` 设置。

我们已完成 `test.txt` 的使用，因此现在可以删除该文件。我们将在其他练习中使用 `media_example` 视图和模板，因此保留它们。在下一节中，我们将讨论如何使用 Web 浏览器上传文件以及 Django 如何在视图中访问它们。

## 使用 HTML 表单进行文件上传

在 *第 6 章，表单* 中，我们学习了 HTML 表单。我们讨论了如何使用 `<form>` 的 method 属性进行 GET 或 POST 请求。到目前为止，我们仅使用表单提交文本数据，但也可以使用表单提交一个或多个文件。

提交文件时，我们必须确保表单上至少有两个属性：method 和 enctype。你可能仍需要其他属性，例如 action。支持文件上传的表单可能如下所示：

```
```html
<form method="post" enctype="multipart/form-data">
```
```

文件上传仅适用于 POST 请求。GET 请求无法实现，因为无法通过 URL 发送文件的全部数据。enctype 属性必须设置为让浏览器知道应将表单数据作为多部分发送，一部分用于表单的文本数据，单独的部分用于附加到表单的每个文件。这种编码对用户是无缝的，他们不知道浏览器如何编码表单，也无需做任何不同的操作。

要将文件附加到表单，你需要创建一个 `file` 类型的输入。你可以手动编写 HTML 代码如下：

```
```html
<input type="file" name="file-upload-name">
```
```

当输入在浏览器中渲染时，空时看起来像这样：

![](img/4840a43efd335abd10b93708225692d6_399_0.png)

图 8.6：空文件输入

按钮的标题可能因浏览器而异。

点击 **Browse…** 按钮将显示 *文件打开* 对话框：

![](img/4840a43efd335abd10b93708225692d6_399_1.png)

图 8.7：macOS 上的文件浏览器

选择文件后，文件名会显示在字段中：

![](img/4840a43efd335abd10b93708225692d6_399_2.png)

图 8.8：文件输入中选择了 cover.jpg

*图 8.8* 显示了一个文件输入，其中已选择名为 `cover.jpg` 的文件。

我们简要了解了文件上传字段在浏览器中的外观，让我们开始查看 Django 端的文件上传过程。

## 在视图中处理上传的文件

除了文本数据，如果表单还包含文件上传，Django 将使用 request.FILES 属性填充上传的文件。request.FILES 属性是一个类似字典的对象，其键为赋予文件输入的 name 属性。

在上一节的表单示例中，文件输入的 name 为 file-upload-name。因此，文件在视图中可通过 request.FILES["file-upload-name"] 访问。

request.FILES 包含的文件是类似文件的对象（具体为 django.core.files.uploadedfile.UploadedFile 实例），因此要使用它们，必须读取其数据。例如，要在视图中获取上传文件的内容，可以编写如下代码：

```
content = request.FILES["file-upload-name"].read()
```

更常见的做法是将文件内容写入磁盘。当文件上传时，它们存储在临时位置（如果小于 2.5 MB 则在内存中，否则在磁盘上的临时文件）。要将文件数据存储在已知位置，必须读取内容然后将其写入该位置的磁盘。UploadedFile 具有 chunks 方法，可一次读取文件数据的一个块，以防止一次性读取整个文件而使用过多内存。

因此，不要简单地使用 read 和 write 函数，使用 chunks 方法一次仅将文件的小块读入内存：

```
with open("/path/to/output.jpg", "wb+") as output_file:
    uploaded_file = request.FILES["file-upload-name"]
    for chunk in uploaded_file.chunks():
        output_file.write(chunk)
```

请注意，在接下来的一些示例中，我们将此代码称为 save_file_upload 函数。假设该函数定义如下：

```
def save_file_upload(upload, save_path):
    with open(save_path, "wb+") as output_file:
        for chunk in upload.chunks():
            output_file.write(chunk)
```

之前的示例代码然后可以重构为调用该函数：

```
uploaded_file = request.FILES["file-upload-name"]
save_file_upload(uploaded_file, "/path/to/output.jpg")
```

