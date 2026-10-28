

```
m.save()  # save the model instance, also write the
            files to disk
        return redirect("/success-page/")
    else:
        form = ExampleModelForm()
    return (request, "template.html", {"form": form})
```

调用模型实例的 save 方法会将模型数据保存到数据库，并将上传的文件保存到磁盘。在下一个练习中，我们将从练习 8.06 – FileField 和 ImageField on models 中创建的 ExampleModel 构建 ModelForm，然后使用它测试文件上传。

## 练习 8.07 – 使用 ModelForm 进行文件和图片上传

在本练习中，你将把 UploadForm 更新为 ModelForm 的子类，并使其自动从 ExampleModel 构建。然后你将更改 media_example 视图，使其从表单自动保存实例，以便可以看到代码量如何减少：

- 1. 在 PyCharm 中打开 media_example 应用的 forms.py 文件。你需要在本章中使用 ExampleModel，因此在文件顶部在 from django import forms 语句之后使用 import 导入它。插入以下行：

```
from .models import ExampleModel
```

- 2. 将 UploadForm 改为 forms.ModelForm 的子类。删除类体并用 class Meta 定义替换，其 model 应为 ExampleModel。将 fields 属性设置为 __all__。完成此步骤后，UploadForm 应如下所示：

```
class UploadForm(forms.ModelForm):
    class Meta:
        model = ExampleModel
        fields = "__all__"
```

保存并关闭文件。它现在应如下所示：https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.07/media_project/media_example/forms.py。

- 3. 打开 media_example 应用的 views.py 文件。由于你不再需要直接引用 ExampleModel，可以删除文件顶部的导入。删除以下行：

```
from .models import ExampleModel
```

- 4. 在 media_example 视图中，删除 form.is_valid() 分支的全部内容并替换为单行：

```
instance = form.save()
```

表单的 save 方法将处理将实例持久化到数据库并保存文件。它将返回 ExampleModel 的实例，与我们在第 7 章 Advanced Form Validation and Model Forms 中处理过的其他 ModelForm 相同。

完成此步骤后，你的 media_example 函数应如下所示 https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.07/media_project/media_example/views.py。保存并关闭 views.py。

- 5. 如果尚未运行，请启动 Django 开发服务器，然后导航到 http://127.0.0.1:8000/media-example/。你应该会看到表单渲染出两个字段，Image 字段和 File 字段（图 8.24）：

![](img/4840a43efd335abd10b93708225692d6_439_0.png)

图 8.24：UploadForm 作为 ModelForm 在浏览器中渲染

请注意，这些字段的名称现在与模型匹配而不是表单，因为表单只是使用模型的字段。

- 6. 浏览并选择一张图片和一个文件（图 8.25），然后提交表单：

![](img/4840a43efd335abd10b93708225692d6_440_0.png)

图 8.25：已选择图片和文件

- 7. 页面将重新加载，并且与 *练习 8.06 – FileField and ImageField on models* 相同，你将看到之前上传的图片（图 8.26）：

![](img/4840a43efd335abd10b93708225692d6_440_1.png)

图 8.26：上传后显示图片

- 8. 最后，检查 `media` 目录的内容。你应该会看到目录结构与 *练习 8.06 – FileField and ImageField on models* 匹配，图片在 `images` 目录中，文件在 `files` 目录中：

![](img/4840a43efd335abd10b93708225692d6_441_0.png)

图 8.27：上传的文件目录与练习 6 匹配

在本练习中，我们将 `UploadForm` 改为 `ModelForm` 子类，这使我们能够自动生成上传字段。我们可以用对表单 `save` 方法的调用替换存储上传文件到模型的代码。

在继续本章的活动之前，我们需要简要讨论在使用实例时如何处理文件保存。

### 处理文件保存

正如你在 *第 7 章* 中使用 `ModelForms` 时所知，你可以向 `ModelForm` 传递 instance 参数，使其在保存时更新现有模型而不是创建新模型。

例如，按如下方式更新视图中的示例模型：

```
```python
def update_view(request, pk):
    instance = ExampleModel.objects.get(pk=pk)
    form = UploadForm(request.POST, request.FILES,
                      instance=instance)
    if form.valid():
        form.save()
```
```

当访问表单的 `cleaned_data` 属性时，我们会得到 `request.POST` 中包含的数据。如果该键没有值，则我们会从实例中获取值。

这通常不会引起任何问题，但在处理 `ImageField` 或 `FileField` 时，返回的数据类型会与请求或实例不同。

如果上传了文件，则 cleaned data 将为 InMemoryUploadedFile 或 TemporaryUploadedFile（取决于其大小；只有小文件才能放入内存）。如果未上传文件，则类型将为 FileField 的 FieldFile 或 ImageField 的 ImageFieldFile。

为了解释，请考虑以下代码：

```
def update_view(request, pk):
    instance = ExampleModel.objects.get(pk=pk)
    form = UploadForm(request.POST, request.FILES,
        instance=instance)
    if form.is_valid()
        form_file = form.cleaned_data["file_field"]
        form_image = form.cleaned_data["image_field"]
```

在上传了数据的情况下（即用户在表单中选择了图片和文件），则 form_file 和 form_image 将为 InMemoryUploadedFile 或 TemporaryUploadedFile。但是，如果用户未为字段上传任何内容（假设表单中字段不是必需的），则 form_file 将为 FieldFile 实例，而 form_image 将为 ImageFieldFile 实例。

为了区分它们，你可以进行几种不同的检查。其中最简单的方法是检查 path 属性的存在。由于上传的文件尚未保存到磁盘，因此它们没有 path：

```
if hasattr(form_file, "path"):
    print("This is a FieldFile")
else:
    print("This is an uploaded file")
```

有时你需要在执行某些操作之前检查正在处理的文件类型，并且如果用户未上传新文件则跳过该操作。例如，我们不希望调整已分配给模型的图片大小。我们只会对新上传的图片进行图片调整大小：

```
if hasattr(image_file, "path"):
    print("This image was on the instances so no need to resize")
else:
    perform_resize(image_file)
```

你需要在本章的第一个活动中进行类似的检查。

我们现在已经涵盖了开始使用文件上传增强 Bookr 所需的一切。在本章的活动中，我们将为书籍添加封面图片和示例文档（PDF、文本文件等）的上传支持。书籍封面将在保存前使用 PIL 调整大小。

## 活动 8.01 – 书籍的图片和 PDF 上传

在本活动中，你将首先清理（删除）我们在本章练习中使用的示例视图、模板、表单、模型和 URL 映射。然后你需要生成并应用迁移以从数据库中删除 ExampleModel。

然后你可以开始添加 Bookr 增强功能，首先向 Book 模型添加 ImageField 和 FileField 以存储书籍封面和示例。然后你将创建迁移并应用它以将这些字段添加到数据库。你可以然后构建一个表单，仅显示这些新字段。你将添加一个视图，使用此表单在先调整图片大小为缩略图大小后保存模型实例及其上传的文件。你可以重用第 7 章 Advanced Form Validation and Model Forms 中的 instance-form.html 模板，只需进行少量更改以允许文件上传。

完成活动的步骤如下：

- 1. 更新 Django 设置以添加 MEDIA_ROOT 和 MEDIA_URL 设置。
- 2. /media/ URL 映射应添加到主 urls.py 文件中。使用 static 视图并利用 Django 设置中的 MEDIA_ROOT 和 MEDIA_URL。请记住，此映射只有在 DEBUG 为 True 时才应添加。
- 3. 向 Book 模型添加 ImageField（命名为 cover）和 FileField（命名为 sample）。字段应分别上传到 book_covers/ 和 book_samples/。它们都应允许 null 和 blank 值。
- 4. 再次运行 makemigrations 和 migrate 以将 Book 模型更改应用到数据库。
- 5. 创建 BookMediaForm 作为 ModelForm 的子类。其 model 应为 Book，fields 仅应为你在步骤 3 中添加的字段。
- 6. 添加 book_media 视图。这将不允许你创建 Book，而是只允许你为现有 Book 添加媒体（因此它必须将 pk 作为必需参数）。
- 7. book_media 视图应验证表单并使用 save 保存它但不提交实例。上传的封面应首先使用 thumbnail 方法调整大小，如 Writing PIL images to ImageField 部分所示。最大尺寸应为 300 px x 300 px。然后应将其存储在实例上，并保存实例。请记住 cover 字段不是必需的，因此在尝试操作图片之前应检查这一点，并且如果实例上已有图片则无需调整图片大小。

