

例如，我们有两个表，即 Book 和 Contributor，我们需要在其中存储书籍的贡献者类型信息，例如 Author、Co-Author 或 Editor。然后贡献者类型存储在名为 BookContributor 的中间表中。以下是 BookContributor 表/模型的示例。请确保将此模型包含在 reviews/models.py 中：

```python
class BookContributor(models.Model):
    class ContributionRole(models.TextChoices):
        AUTHOR = "AUTHOR", "Author"
        CO_AUTHOR = "CO_AUTHOR", "Co-Author"
        EDITOR = "EDITOR", "Editor"

    book = models.ForeignKey(
        Book, on_delete=models.CASCADE)
    contributor = models.ForeignKey(
        Contributor, on_delete=models.CASCADE)
    role = models.CharField(
        verbose_name="The role this contributor had in the book.",
        choices=ContributionRole.choices,
        max_length=20)
```

> **注意**
> 完整的 models.py 文件可以在此处查看：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter02/final/bookr/reviews/models.py。

像 BookContributor 这样的中间表通过使用外键同时指向 Book 和 Contributor 表来建立关系。它还可以具有额外的字段，可以存储有关 BookContributor 模型与以下字段的关系的信息：

- **book**：这是对 Book 模型的外键。如前所述，`on_delete=models.CASCADE` 会在相关书籍从应用程序中删除时删除关系表中的条目。
- **Contributor**：同样，这是对 Contributor 模型/表的外键。这在删除时也定义为 CASCADE。
- **role**：这是中间模型的字段，用于存储有关 Book 和 Contributor 之间关系的额外信息。
- **class ContributionRole(models.TextChoices)**：这可以通过创建 `models.TextChoices` 的子类来定义一组选择。例如，ContributionRole 是从 TextChoices 创建的子类，由 roles 字段使用以将 Author、Co-Author 和 Editor 定义为一组选择。
- **`choices`**：这是指在模型中定义的一组选择，在使用模型创建 Django 表单时很有用。

> **注意**
> 当在建立多对多关系时未提供 `through` 字段选项时，Django 会自动创建一个中间表来管理该关系。

## 一对一关系

在此关系中，表中的一条记录将仅引用不同表中的一条记录。例如，一个人只能有一个驾驶执照，因此拥有驾驶执照的人可以形成一对一的关系：

![](img/4840a43efd335abd10b93708225692d6_112_0.png)

图 2.15：一对一关系的示例

OneToOneField 可用于建立一对一关系，如下所示：

```python
class DriverLicence(models.Model):
    person = models.OneToOneField(
        Person, on_delete=models.CASCADE)
    licence_number = models.CharField(max_length=50)
```

现在我们已经探索了数据库关系，让我们回到我们的 bookr 应用程序并在其中添加一个模型。

## 添加 Review 模型

我们已经将 Book 和 Publisher 模型添加到 reviews/models.py 文件中。我们将要添加的最后一个模型是 Review 模型。以下代码片段可以帮助我们完成此操作：

```python
from django.contrib import auth

class Review(models.Model):
    content = models.TextField(
        help_text="The Review text.")
    rating = models.IntegerField(
        help_text="The rating the reviewer has given.")
    date_created = models.DateTimeField(
        auto_now_add=True,
        help_text="The date and time the review was created.")
    date_edited = models.DateTimeField(
        null=True,
        help_text="The date and time the review was last edited."
    )
    creator = models.ForeignKey(
        auth.get_user_model(), on_delete=models.CASCADE)
    book = models.ForeignKey(
        Book, on_delete=models.CASCADE,
        help_text="The Book that this review is for."
    )
```

> **注意**
> 
> 完整的 `models.py` 文件可以在此处查看：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter02/final/bookr/reviews/models.py。

review 模型/表将用于存储用户提供的书籍评论和评分。它具有以下字段：

- **content**：此字段存储书籍评论的文本；因此使用的字段类型为 `TextField`，因为它可以存储大量文本。
- **rating**：此字段存储书籍的评论评分。由于评分将为整数，因此使用的字段类型为 `IntegerField`。
- **date_created**：此字段存储评论撰写的时间和日期；因此字段类型为 `DateTimeField`。
- **date_edited**：此字段存储每次编辑评论时的日期和时间。同样，字段类型为 `DateTimeField`。
- **Creator**：此字段指定评论创建者或撰写书籍评论的人。请注意，这是一个指向 `auth.get_user_model()` 的外键，它指的是 Django 内置身份验证模块中的 User 模型。它具有 `on_delete=models.CASCADE` 字段选项。这说明当用户从数据库中删除时，该用户撰写的所有评论都将被删除。
- **Book**：Reviews 具有一个名为 book 的字段，是对 Book 模型的外键。这是因为评论必须为书籍评论应用程序撰写，并且一本书可以有许多评论，所以这是一对多的关系。这也使用 `on_delete=models.CASCADE` 字段选项定义，因为一旦书籍被删除，就没有保留评论的意义了。因此，当一本书被删除时，所有引用该书的评论也将被删除。

在下一节中，我们将学习并实现模型方法。

## 模型方法

在 Django 中，我们可以在模型类中编写方法。这些被称为 **模型方法**，它们可以是自定义方法或覆盖 Django 模型默认方法的特殊方法。其中一种方法就是 `__str__()`。此方法返回模型实例的字符串表示形式，在使用 Django shell 时尤其有用。在以下示例中，`__str__()` 方法被添加到 Publisher 模型中，Publisher 对象的字符串表示形式将为出版商的名称；因此返回 `self.name`，其中 `self` 指的是 Publisher 对象：

```python
class Publisher(models.Model):
    """A company that publishes books."""
    name = models.CharField(
        max_length=50,
        help_text="The name of the Publisher.")
    website = models.URLField(
        help_text="The Publisher's website.")
    email = models.EmailField(
        help_text="The Publisher's email address.")

    def __str__(self):
        return self.name
```

同样将 `__str__()` 方法添加到 Contributor 和 Book 中，如下所示：

```python
class Book(models.Model):
    """A published book."""
    title = models.CharField(
        max_length=70, help_text="The title of the book.")
    publication_date = models.DateField(
        verbose_name="Date the book was published.")
    isbn = models.CharField(
        max_length=20,
        verbose_name="ISBN number of the book.")
    publisher = models.ForeignKey(
        Publisher,
        on_delete=models.CASCADE)
    contributors = models.ManyToManyField(
        'Contributor', through="BookContributor")

    def __str__(self):
        return self.title


class Contributor(models.Model):
    """A contributor to a Book, e.g. author, editor, co-author."""
    first_names = models.CharField(
        max_length=50,
        help_text="The contributor's first name or names.")
    last_names = models.CharField(
        max_length=50,
        help_text="The contributor's last name or names.")
    email = models.EmailField(
        help_text="The contact email for the contributor.")

    def __str__(self):
        return self.first_names
```

同样，book 的字符串表示形式是 title，因此返回值为 `self.title`，其中 `self` 指的是 Book 对象。Contributor 的字符串表示形式是贡献者的名字；因此返回 `self.first_names`。这里，`self` 指的是 Contributor 对象。接下来，我们将查看迁移 reviews 应用。

## 迁移 reviews 应用

由于我们已经准备好了整个模型文件，让我们现在将模型迁移到数据库中，类似于我们之前对已安装应用所做的操作。由于 reviews 应用有一组由我们创建的模型，在运行迁移之前创建迁移脚本非常重要。迁移脚本有助于识别模型中的任何更改，并在运行迁移时将这些更改传播到数据库中。按照以下步骤创建迁移脚本，然后将模型迁移到数据库中：

1. 执行以下命令创建迁移脚本：

```bash
python manage.py makemigrations reviews
```

你应该会得到类似以下的输出：

```text
reviews/migrations/0002_auto_20191007_0112.py
  - Create model Book
  - Create model Contributor
  - Create model Review
  - Create model BookContributor
  - Add field contributors to book
  - Add field publisher to book
```

迁移脚本将在应用程序文件夹中的名为 migrations 的文件夹中创建。

2. 接下来，使用 migrate 命令将所有模型迁移到数据库中：

```bash
python manage.py migrate reviews
```

