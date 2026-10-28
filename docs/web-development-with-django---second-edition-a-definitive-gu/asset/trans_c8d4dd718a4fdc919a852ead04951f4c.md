

Django 会打开正确的路径，并在响应后关闭它。Django 还会尝试确定文件的正确 MIME 类型。我们假设 FileField 的 upload_to 属性已设置为受保护的目录，Web 服务器禁止直接访问该目录。

## 在 FileField 中存储现有文件或内容

我们已经看到如何将上传的文件存储到图像字段中：只需像这样将其分配给字段即可：

```python
m.file_field = request.FILES["file_upload"]
```

但是我们如何将字段值设置为我们可能已经在磁盘上拥有的现有文件呢？你可能会认为可以使用标准的 Python 文件对象，但这不起作用：

```python
m.file_field = open("/path/to/file.txt", "rb")  # Don't do this
```

你也可能尝试使用一些内容来设置文件：

```python
m.file_field = "new file content"  # Don't do this
```

这同样不会起作用。

你需要改用 FileField 的 save 方法，该方法接受 Django File 对象或 ContentFile 对象的实例（这两个类的完整路径分别是 django.core.files.File 和 django.core.files.base.ContentFile）。我们将简要讨论 save 方法及其参数，然后再回到这些类。

FileField 的 save 方法接受三个参数：

- name：这是你要保存的文件的名称，也是文件在存储引擎中保存时的名称（在我们的例子中，是保存在磁盘上的 MEDIA_ROOT 内部）。
- content：这是一个我们刚刚看到的 File 或 ContentFile 的实例；同样，我们很快会讨论这些。
- save：此参数是可选的，默认为 True。它表示在保存文件后是否将模型实例保存到数据库。如果设置为 False（即模型未保存），则文件仍会写入存储引擎（到磁盘），但关联不会存储在模型上。之前的 file 路径（如果未设置则无文件）仍会存储在数据库中，直到手动调用模型实例的 save 方法。你只有在打算对模型实例进行其他更改然后手动保存它时，才应将此参数设置为 False。

回到 `File` 和 `ContentFile`：要使用哪一个取决于你想在 `FileField` 中存储什么。

`File` 用作 Python 文件对象的包装器，如果你有一个现有的文件或类似文件的对象想要保存，就应该使用它。类似文件的对象包括 `io.BytesIO` 或 `io.StringIO` 实例。要实例化一个 `File` 实例，只需将原生文件对象传递给构造函数，例如：

```python
f = open("/path/to/file.txt", "rb")
file_wrapper = File(f)
```

当你已经加载了一些数据时使用 `ContentFile`，数据可以是 `str` 对象或 `bytes` 对象。将数据传递给 `ContentFile` 构造函数：

```python
string_content = ContentFile("A string value")
bytes_content = ContentFile(b"A bytes value")
```

现在你有了 `File` 实例或 `ContentFile` 实例，将数据保存到 `FileField` 很简单，使用 `save` 方法：

```python
m = ExampleModel.objects.first()
with open("/path/to/file.txt") as f:
    file_wrapper = File(f)
    m.file_field.save("file.txt", file_wrapper)
```

由于我们没有为 save 方法传递 `save` 的值，它将默认为 `True`，因此模型实例会自动持久化到数据库中。

接下来，我们将查看如何将使用 PIL 处理过的图像保存回图像字段。

## 将 PIL 图像写入 ImageField

在 *Exercise 8.05 – image uploads using Django forms* 中，你使用 PIL 调整图像大小并将其保存到磁盘。当使用模型时，你可能希望执行类似的操作，但让 Django 使用 `ImageField` 处理文件存储，这样你就不必手动操作了。如练习中所示，你可以将图像保存到磁盘，然后使用 `File` 类包装存储的路径——像这样：

```python
image = Image.open(request.FILES["image_field"])
image.thumbnail((150, 150))
image.save("/tmp/thumbnail.jpg")  # save thumbnail to temp
                             # location

with open("/tmp/thumbnail.jpg", "rb") as f:
    image_wrapper = File(f)
    m.image_field.save("thumbnail.jpg", image_wrapper)

os.unlink("/tmp/thumbnail.jpg")  # clean up temp file
```

在这个例子中，我们让 PIL 使用 `Image.save()` 方法将图像存储到临时位置，然后重新打开文件。

这种方法有效，但并不理想，因为它涉及将文件写入磁盘然后再次读出，有时速度会很慢。相反，我们可以在内存中执行整个过程。

> **Note**
>
> `io.BytesIO` 和 `io.StringIO` 是有用的对象。它们的行为像文件，但只存在于内存中。BytesIO 用于存储原始字节，StringIO 接受 Python 3 的原生 Unicode 字符串。你可以使用 read、write 和 seek，就像普通文件一样。与普通文件不同，它们不会写入磁盘，而是在程序终止时消失，或超出作用域后被垃圾回收。它们非常有用，如果一个函数想要写入类似文件的内容，但你希望立即访问数据。

首先，我们将图像数据保存到 `io.BytesIO` 对象中。然后，我们将 BytesIO 对象包装在 `django.core.files.images.ImageFile` 实例中（File 的子类，专门用于图像，并提供 width 和 height 属性）。一旦我们有了这个 ImageFile 实例，我们就可以在 ImageField 的 save 方法中使用它。

> **Note**
>
> `ImageFile` 是一个文件或类似文件的包装器，就像 `File` 一样。它提供了两个额外的属性：width 和 height。ImageFile 不会在你用它包装非图像时产生任何错误。例如，你可以使用 `open()` 打开一个文本文件并将文件句柄传递给 ImageFile 构造函数而不会有问题。你可以通过尝试访问 width 或 height 属性来检查你传入的图像文件是否有效：如果这些值为 None，则 PIL 无法解码图像数据。你可以自行检查这些值的有效性，如果它们为 None 则抛出异常。

让我们在一个视图中实际看看这一点：

```python
from io import BytesIO
from PIL import Image
from django.core.files.images import ImageFile


def index(request, pk):
    # trim out logic for checking if method is POST

    # get a Model instance, or create a new one
    m = ExampleModel.objects.get(pk=pk)

    # store the uploaded image in a variable for shorter code
    uploaded_image = request.FILES["image_field"]

    # load a PIL image instance from the uploaded file
    image = Image.open(uploaded_image)

    # perform the image resize
    image.thumbnail((150, 150))

    # Create a BytesIO file-like object to store
    image_data = BytesIO()

    # Write the Image data back out to the BytesIO object
    # Retain the existing format from the uploaded image
    image.save(fp=image_data, format=uploaded_image.format)

    # Wrap the BytesIO containing the image data
    image_file = ImageFile(image_data)

    # Save the wrapped image file data with the original name
    m.image_field.save(uploaded_image.name, image_file)
    # this also saves the model instance
    return redirect("/success-url/")
```

你可以看到这段代码稍微多一点，但它节省了将数据写入磁盘的操作。你可以根据需要选择使用任一方法（或你自己想出的其他方法）。

## 在模板中引用媒体

一旦我们上传了文件，我们希望能够在模板中引用它。对于上传的图像，例如书籍封面，我们希望在页面上显示图像。我们在 Exercise 8.02 – template settings, and using MEDIA_URL in templates 中看到如何使用 MEDIA_URL 在模板中构建 URL。当在模型实例上使用 FileField 或 ImageField 时，不必这样做，因为 Django 已为你提供了此功能。

FileField 的 url 属性将根据 settings 中的 MEDIA_URL 自动生成媒体文件的完整 URL。

> **Note**
>
> 注意，本节中对 FileField 的引用也适用于 ImageField，因为它是 FileField 的子类。

