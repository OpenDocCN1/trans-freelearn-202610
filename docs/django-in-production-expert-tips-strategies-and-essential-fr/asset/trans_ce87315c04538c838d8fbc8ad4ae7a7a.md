

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'HOST': 'tiny.db.elephantsql.com',
        'NAME': 'wcgzvqra',
        'USER': 'wcgzvqra',
        'PORT': 5432,
        'PASSWORD': 'lyuX6vWvAtrqZSY1oiPKNBMoKFkctBVn',
    }
}
```

添加 PostgreSQL 配置后，在终端中运行 `python manage.py runserver`。这会给出一个长错误，提示 `No module named 'psycopg2' is found`。要修复此问题，Django 需要一个额外的包 `psycopg2`，它是 Python 的 PostgreSQL 适配器。

本书中我们不会使用 `psycopg2`，因为它需要使用源代码构建。相反，我们将使用 `binary`，因为它更推荐给初学者。让我们使用以下命令安装此包：

```
pip install psycopg2-binary
```

这将在我们的本地 Python 环境中安装 PostgreSQL 客户端包。

重要说明

在开发期间使用 `psycopg2-binary`，但在生产环境中尽量避免使用。有关更多信息，请参阅 [https://www.psycopg.org/docs/install.html#psycopg-vs-psycopg-binary](https://www.psycopg.org/docs/install.html#psycopg-vs-psycopg-binary)。

现在，再次运行 `python manage.py runserver` 命令；我们应该会看到默认的起飞屏幕，就像我们在浏览器中打开 `http://127.0.0.1:8000` 时看到的那样（就像我们在 [*Chapter 1*](#B18867_01.xhtml#_idTextAnchor014) 中看到的那样）。

重要说明

在 `settings.py` 文件中硬编码数据库或任何项目相关配置并不是推荐或最优的方式。我们应该使用环境变量来执行此类操作。我们将在 [*Chapter 11*](#B18867_11.xhtml#_idTextAnchor219) 中使用 Docker 时学习更多关于使用环境变量获取项目配置的内容。

现在我们已将 PostgreSQL 集成到 Django 项目中，让我们学习如何使用模型和 Django ORM。

# 使用模型和 Django ORM

对于任何组织而言，数据都是 Django 中最重要的基础元素之一，而 `models.py` 文件用于结构化数据库中数据的存储方式。一个架构良好的系统将拥有坚实的 Django 模型基础，在此基础上，开发者在创建数据库模式之前花费了足够的时间并进行了适当的调研。

开发者在设计模式时最常见的问题之一是是否执行规范化或反规范化。答案始终是“视情况而定”。它取决于多个因素，例如数据访问模式和我们试图解决的性能瓶颈。因此，作为经验法则，始终更倾向于先从规范化的数据库模式设计开始，然后随着我们发现不同的用例，尝试通过引入缓存或索引来解决问题，最后再进行反规范化。

重要说明

我们不会关注如何使用 Django 模型的基础知识，或如何使用 Django ORM 编写查询。如果您不熟悉 Django 模型的基础知识，请阅读官方网站上的 Django 教程或阅读官方文档 [https://docs.djangoproject.com/en/stable/ref/models/querysets/](https://docs.djangoproject.com/en/stable/ref/models/querysets/)。

我们不会介绍 Django ORM 的基本概念，但在下一节中，我们将把 Django 模型集成到博客应用中。

## 添加 Django 模型

对于不同的博客应用，我们可以根据功能拥有不同类型的数据库模型。在我们的应用中，我们将从 `blog` 和 `author` 等基本功能开始。我们将创建单独的 Django 应用来添加这些模型：

```
python manage.py startapp author
python manage.py startapp blog
```

现在，将应用添加到 `settings.py` 中的 `CUSTOM_APPS` 部分：

```
CUSTOM_APPS = [
    'blog',
    'author',
]
```

现在，前往 `author/models.py` 文件并添加 `Author` 模型：

```
class Author(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField()
    bio = models.TextField()
    def __str__(self):
        return self.name
```

同样，将 `Blog` 模型添加到 `blog/models.py` 文件中：

```
class Blog(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    author = models.ForeignKey('author.Author', on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    def __str__(self):
        return self.title
```

现在，运行迁移命令以创建迁移，并将迁移应用到数据库：

```
python manage.py makemigrations
python manage.py migrate
```

至此，我们已成功使用 Django ORM 添加了基本数据库模型。现在，让我们讨论一些关于不同 Django ORM 功能的重要要点，以便我们更好地了解如何高效地使用 Django。

## 基本 ORM 概念

在本节中，我们将学习不同的 ORM 概念，帮助 Django 开发者高效地使用 ORM 实现业务逻辑。

### Null 与 blank

创建新模型字段时，`null=True` 表示允许该列存储 `null` 值，而 `blank=True` 表示 Django 管理后台将允许该字段以空字符串作为有效值。简而言之，`null=True` 是数据库约束，而 `blank=True` 是 Django 应用约束。

我们也可以同时使用 `null` 和 `blank`。例如，我们可以允许 `Author` 模型的 `bio` 字段同时为 `null` 和 `blank`：

```
class Author(models.Model):
    ...
    bio = models.TextField(null=True, blank=True)
```

### auto_now 与 auto_now_add

`DateTime` 字段有两个选项，允许开发者自动将当前时间保存到数据库字段中：

*   `auto_now` 在有人编辑给定行时，用当前 `DateTime` 字段更新值
*   `auto_now_add` 仅在对象创建时添加当前时间戳

现在，一个重要问题是，我们应该在什么时候使用它们？作为基本经验法则，我建议使用 `auto_now_add` 来捕获 *created at* 时间戳，并使用 `auto_now` 来捕获最后的 *updated at* 时间戳。

让我们学习如何在 Django ORM 中使用这些选项：

```
class Blog(models.Model):
    ...
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    ..
```

### 避免使用原始 SQL 查询

Django ORM 是数据库查询的包装器，因此，有时开发者会想直接添加 SQL 查询逻辑。Django 为我们提供了不同的接口，例如 `connection.cursor` 上下文管理器和 `<Model>.objects.raw()` 模型 ORM 接口，通过 Django 数据库连接执行原始查询。尽量避免在 Django 中使用原始 SQL 查询，并且除非必要，请确保在应用代码中有足够的检查以防止 SQL 注入。

### 使用查询表达式和数据库函数

查询表达式是开发者因学习曲线而避免使用的一种最被低估的功能。在构建应用逻辑时，您应始终尝试在 SQL 中执行所有数学和逻辑操作。例如，如果应用用例需要我们查找员工的平均薪资，我们应避免在 Python 中编写逻辑；相反，我们应使用 Django ORM 的 `aggregate` 来执行此操作，例如 `<model>.objects.aggregate(Avg('salary'))`。

重要说明

Django 文档提供了大量查询表达式和数据库函数的示例，例如 *aggregate*、*F* 和 *window* 函数，它们有助于在数据库层执行计算。这比在 Python 中编写的常规逻辑更快、更具可扩展性。

