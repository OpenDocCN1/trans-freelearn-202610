

这可以在你能够访问实例和字段的任何地方使用，例如在视图或模板中。以下示例在视图中：

```
instance = ExampleModel.objects.first()
url = instance.file_field.url  # Get the URL
```

或在模板中（假设实例已传递到模板上下文）：

```
<img src="{{ instance.file_field.url }}">
```

在下一个练习中，我们将创建一个带有 FileField 和 ImageField 的新模型，然后展示 Django 如何自动保存这些文件。我们还将演示如何检索已上传文件的 URL。

## 练习 8.06 – 模型上的 FileField 和 ImageField

在本练习中，我们将创建一个带有 FileField 和 ImageField 的模型。完成此操作后，我们需要生成迁移并应用它。我们将更改一直在使用 UploadForm，使其同时具有 FileField 和 ImageField。media_example 视图将被更新以将上传的文件存储到模型实例上。最后，我们将在示例模板中添加 <img> 以显示之前上传的图像：

- 1. 在 PyCharm 中，打开 media_example 应用的 models.py 文件。创建一个名为 ExampleModel 的新模型，包含两个字段：名为 image_field 的 ImageField，以及名为 file_field 的 FileField。ImageField 应将 upload_to 设置为 images/，FileField 应将 upload_to 设置为 files/。完成的模型应如下所示：

```
class ExampleModel(models.Model):
    image_field = models.ImageField(upload_to="images/")
    file_field = models.FileField(upload_to="files/")
```

你的 models.py 文件现在应如下所示：https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.06/media_project/media_example/models.py。

- 2. 打开终端并导航到 media_project 项目目录。确保你的 bookr 虚拟环境处于激活状态。运行 makemigrations 管理命令为此新模型生成迁移（对于 Windows，你可以在以下代码中使用 python 而不是 python3）：

```
python3 manage.py makemigrations
```

> Note
> 要了解如何创建和激活虚拟环境，请参阅本书的 Preface 部分。

输出应如下所示：

```
(bookr)$ python3 manage.py makemigrations
Migrations for 'media_example':
    media_example/migrations/0001_initial.py
      - Create model ExampleModel
```

3. 通过运行 migrate 管理命令应用迁移：

```
python3 manage.py migrate
```

输出将如下所示：

```
(bookr)$ python3 manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes,
  reviews, sessions
Running migrations:
  # output trimmed for brevity
  Applying media_example.0001_initial... OK
```

请注意，所有初始 Django 迁移也将被应用，因为我们在创建项目后没有应用这些迁移。

4. 切回 PyCharm 并打开 media_example 应用的 forms.py 文件。将现有的 ImageField 从 file_upload 重命名为 image_upload。然后，添加一个名为 file_upload 的新 FileField。进行这些更改后，你的 UploadForm 代码应如下所示：

```
class UploadForm(forms.Form):
    image_upload = forms.ImageField()
    file_upload = forms.FileField()
```

你可以保存并关闭文件。它应如下所示：https://raw.githubusercontent.com/PacktPublishing/Web-Development-with-Django-Second-Edition/main/Chapter08/Exercise8.06/media_project/media_example/forms.py。

5. 打开 media_example 应用的 views.py 文件。首先，将 ExampleModel 导入到文件中。为此，在现有导入语句之后在文件顶部添加这一行：

```
from .models import ExampleModel
```

一些导入将不再需要，因此你可以删除这些行：

```
from PIL import Image
from django.conf import settings
```

10. 如果尚未运行，请启动 Django 开发服务器，然后导航到 `http://127.0.0.1:8000/media-example/`。你应该会看到渲染出包含两个字段的表单：

![](img/4840a43efd335abd10b93708225692d6_434_0.png)

图 8.20：包含两个字段的 UploadForm

11. 为每个字段选择一个文件 – 对于 `ImageField`，你必须选择一张图片，但对于 `FileField` 允许任何类型的文件。图 8.20 显示了选择文件后的字段：

![](img/4840a43efd335abd10b93708225692d6_434_1.png)

图 8.21：选择文件后的 ImageField 和 FileField

然后，提交表单。如果提交成功，页面将重新加载，并且我们上次上传的图像将被显示（图 8.22）：

![](img/4840a43efd335abd10b93708225692d6_435_0.png)

图 8.22：显示上次上传的图像

12. 你可以通过查看 MEDIA_ROOT 目录来了解 Django 如何存储文件。图 8.23 显示了 PyCharm 中的目录结构：

![](img/4840a43efd335abd10b93708225692d6_435_1.png)

图 8.23：Django 创建的已上传文件

你可以看到 Django 已创建了 `files` 和 `images` 目录。这些是你设置在模型 `ImageField` 和 `FileField` 的 `upload_to` 参数中的目录。

我们也可以通过尝试下载它们来验证这些上传，例如，`http://127.0.0.1:8000/media/files/sample.txt` 或 `http://127.0.0.1:8000/media/images/cover.jpg`。

在本练习中，我们创建了带有 FileField 和 ImageField 的 ExampleModel，并了解了如何将上传的文件存储在其上。我们了解了如何为模板生成已上传文件的 URL。我们尝试上传了一些文件，并看到 Django 自动创建了 upload_to 目录（media/files 和 media/images），然后将文件存储在其中。

在下一节中，我们将了解如何通过使用 ModelForm 来生成表单并在无需手动在视图中设置文件的情况下保存模型，从而进一步简化流程。

## ModelForms 和文件上传

我们已经看到在表单上使用 `form.ImageField` 可以防止非图像被上传。我们还看到 `models.ImageField` 使为模型存储图像变得容易。但我们需要意识到 Django 并不会阻止你将非图像文件设置为 `ImageField`。例如，考虑一个同时具有 `FileField` 和 `ImageField` 的表单：

```
class ExampleForm(forms.Form):
    uploaded_file = forms.FileField()
    uploaded_image = forms.ImageField()
```

在下面的视图中，如果表单上的 `uploaded_image` 字段不是图像，则表单将不会验证，因此上传数据的某些有效性得以保证。以下是一个示例：

```
def view(request):
    form = ExampleForm(request.POST, request.FILES)
    if form.is_valid():
        m = ExampleModel()
        m.file_field = form.cleaned_data["uploaded_file"]
        m.image_field = 
            forms.cleaned_data["uploaded_image"]
        m.save()
    return render(request, "template.html")
```

由于我们确信表单有效，我们知道 `forms.cleaned_data["uploaded_image"]` 必须包含一张图像。因此，我们永远不会将非图像分配给模型实例的 `image_field`。

然而，如果我们在代码中犯了错误，写了如下内容怎么办：

```
m.image_field = forms.cleaned_data["uploaded_file"]
```

也就是说，如果我们不小心引用了 `FileField`，Django 不会验证是否将（潜在的）非图像分配给 `ImageField`，因此它不会抛出异常或产生任何错误。我们可以通过使用 `ModelForm` 来减轻此类问题的可能性。

我们在 *第 7 章，高级表单验证和模型表单* 中介绍了 `ModelForm`。这些是其字段从模型自动定义的表单。我们看到 `ModelForm` 具有 save 方法，可自动在数据库中创建或更新模型数据。当与具有 `FileField` 或 `ImageField` 的模型一起使用时，`ModelForm` 的 save 方法也会保存上传的文件。

以下是使用 `ModelForm` 在视图中保存新模型实例的示例。在这里，我们只需确保将 `request.FILES` 传递给 `ModelForm` 构造函数：

```
class ExampleModelForm(forms.Model):
    class Meta:
        model = ExampleModel  # The same ExampleModel class
                            # we've seen previously
        fields = "__all__"


def view(request):
    if request.method == "POST":
        form = ExampleModelForm(request.POST,
                                request.FILES)
        form.save()
        return redirect("/success-page")
    else:
        form = ExampleModelForm()
    return (request, "template.html", {"form": form})
```

与任何 `ModelForm` 一样，save 方法可以调用时将 commit 参数设置为 `False`。然后模型实例不会保存到数据库，并且 `FileField` 和 `ImageField` 文件不会保存到磁盘。save 方法应在其模型实例上调用 – 这将提交对数据库的更改并保存文件。在下面的这个简短示例中，我们在保存之前在模型实例上设置一个值：

```
def view(request):
    if request.method == "POST":
        form = ExampleModelForm(request.POST,
                                request.FILES)
        m = form.save(False)
        m.attribute = "value"  # Set arbitrary value on the
                                # model instance before save
