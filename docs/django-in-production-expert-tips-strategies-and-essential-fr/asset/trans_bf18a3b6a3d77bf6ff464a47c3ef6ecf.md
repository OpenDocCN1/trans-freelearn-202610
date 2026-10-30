

现在，运行 `python manage.py makemigrations` 命令；我们将在 `author/migrations` 文件夹中得到一个新的假字段。我们不想将更改应用到数据库中，因此我们将伪造迁移。要伪造我们的迁移，我们将以假迁移的方式执行 `author` 应用 `0002` 的 `migrate` 命令：

```
~ python manage.py migrate author 0002 --fake
Operations to perform:
  Target specific migration: 0002_author_demo_field, from author
Running migrations:
  Applying author.0002_author_demo_field... FAKED
```

这里，`0002` 迁移文件的更改被伪造了——也就是说，没有执行数据库模式更改，但我们仍然向 `django_migrations` 表中添加了一条记录。

现在，要撤消这些假数据库迁移更改，我们可以使用 `migrate` 命令：

```
~ python manage.py migrate author 0001 --fake
Operations to perform:
  Target specific migration: 0001_initial, from author
Running migrations:
  Rendering model states... DONE
  Unapplying author.0002_author_demo_field... FAKED
```

请记住，如果我们在回滚时不使用 `fake` 标志，那么我们将遇到错误，因为我们尝试回滚的数据库迁移从未真正应用到数据库表中。

### 重要说明

在生产环境中使用假迁移更改时请务必小心。只有当数据库模式与现有 Django 模型定义相同时，才应用假迁移。

### 避免为数据迁移创建自定义迁移

Django 提供了创建自定义迁移文件的方法，有时开发人员会想创建自定义迁移文件来执行数据迁移。然而，虽然这是一种很好的做法，但我始终会避免这样做。为什么呢？因为你将在生产环境中运行的数据迁移文件也会在其他开发人员的本地系统上运行，通常数据迁移文件需要数据文件；这会造成问题，因为该迁移文件与生产环境兼容，但与本地和暂存应用不兼容。

### 在重复迁移上创建系统检查

你应该创建系统检查，以防止开发人员合并具有相同迁移文件编号的重复迁移文件。每当 Django 遇到此类重复项时，它会给你提供选择。尽管它们是完美的解决方案，但如果你已配置 CI/CD，那么可能为时已晚，无法获得这些选项。

### 添加新字段

始终在向现有数据库表插入新列时添加默认值。优先添加空值，因为与一些随机值相比，使用空值的数据库迁移所需时间要少得多。

现在我们已经了解了 Django 迁移的工作原理，让我们学习在处理模型和 ORM 时应遵循的最佳实践。

# 探索模型和 ORM 的最佳实践

Django 的官方文档是对如何使用模型和 ORM 的非常详细的指南，它可以帮助你使用 ORM 编写复杂的查询。但当你创建实际应用时，这不仅仅是编写好的查询；还有很多细枝末节是你应该知道的，或者在自己犯错后才了解的。在本节中，我们将学习所有那些小技巧，它们可以帮助我们更好地使用 Django 模型和 ORM。

## 使用基础模型

抽象基础模型是 Django 中最受欢迎的模型继承方式之一。我们应该在发现模型属性重复时创建通用的基础模型。我发现的最有用的基础模型之一是 `TimeStampedBaseModel`，它具有 `created_at` 和 `updated_at` 字段。使用此基础模型创建任何模型将自动添加 `created at` 和 `updated at` 时间戳字段；例如，每当有人创建 `DemoModel` 对象或更新它时，我们将拥有相应的时间戳。让我们看一下实现：

```
from django.db import models
class TimeStampedBaseModel(models.Model):
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    class Meta:
        abstract = True
class DemoModel(TimeStampedBaseModel):
    name = models.CharField(max_length=100)
```

我们应该创建一个通用的 Django 应用，其中将包含所有全局基础模型，例如 `TimeStampedBaseModel`；这将使我们能够在整个项目的不同 Django 应用中使用 `TimeStampedBaseModel`。

## 对任何 DateTime 相关数据使用 timezone.now()

每当你处理任何 `DateTime` 相关数据时，始终使用 `django.utils.timezone.now()` 函数。常规的 Python `datetime.now()` 函数不感知时区，因此它不会在时间戳数据库字段中保存时区信息。然而，`timezone.now()` 为我们提供了时区信息，这将有助于我们在数据库中保存时区。

### 重要说明

Django 设置中有 `TIME_ZONE` 变量，用于定义时区；默认情况下，每个 Django 项目都设置为 UTC，因此你需要将设置配置为相应的时区。你可以在这里了解更多：[https://docs.djangoproject.com/en/stable/topics/i18n/timezones/](https://docs.djangoproject.com/en/stable/topics/i18n/timezones/)。

## 如何避免模型中的循环依赖

`Circular dependency` 是开发人员的噩梦。在成熟的代码库中出现循环依赖很常见，当它发生在模型之间时，就变得难以解决。这发生在我们有两个应用之间存在相互依赖的外键引用时。

例如，查看 *Figure 2.8* 中显示的示例。我们有两个应用模型相互依赖，创建了循环依赖错误：

![Figure 2.8: When two apps have circular dependency due to model reference](image/B18867_02_08.jpg) 

Figure 2.8: When two apps have circular dependency due to model reference

Django 为我们提供了一种方法来避免此循环依赖错误，即通过基于字符串的外键引用。例如，在 *Figure 2.9* 中，`A1Model` 将在外键引用中使用 `app_b.B1Model`，我们无需导入 `app_b` 的模型；同样，`B2Model` 将更新为使用 `app_a.A2Model` 作为字符串引用。在从 `models.py` 文件中删除导入后，就不会再有循环依赖错误，如下所示：

![Figure 2.9: Circular dependency resolved using a string-based reference](image/B18867_02_09.jpg) 

Figure 2.9: Circular dependency resolved using a string-based reference

我们在 `ForeignKey` 字段中添加的模型字符串引用会在 Django 内部自动创建为 `ForeignKey` 引用。

## 为所有模型定义 __str__

在开发阶段，经常需要在 Django shell 上工作，因此使用人类可读的字符串而不是随机 ID 来识别模型对象非常重要。作为示例，对于 author 对象，例如 `<Author: Author object (1)>`，如果我们在 author 模型定义中未定义 `__str__`，我们将得到 *Figure 2.10* 中显示的输出：

![Figure 2.10: The Author object string’s output when we don’t have the __str__ method defined](image/B18867_02_10.jpg) 

Figure 2.10: The Author object string’s output when we don’t have the `__str__` method defined

现在，让我们为 `Author` 模型定义 `__str__` 方法：

```
class Author(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField()
    bio = models.TextField()
    def __str__(self):
        return f'{self.name}-{self.id}'
```

使用此模型，如果我们创建一个新对象，我们将得到 `Author: <name>-<id>` 作为输出，如 *Figure 2.11* 所示，这更易于阅读和操作：

![Figure 2.11: The Author object string’s output when we have the __str__ method defined](image/B18867_02_11.jpg) 

