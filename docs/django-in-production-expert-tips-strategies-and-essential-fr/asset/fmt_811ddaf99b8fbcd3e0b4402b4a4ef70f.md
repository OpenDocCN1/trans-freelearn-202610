
进入`/path/to/xxx`目录，找到`xxx.json`。

## `exclude`

在某些情况下，我们可能希望将模型的所有字段添加到序列化器中，除了少数几个字段；这时 `exclude` 属性非常有用。当我们定义 `exclude` 属性时，除了列表中定义的字段外，所有字段都会被序列化器忽略。例如，在下面的代码片段中，`Blog` 模型中存在的所有字段都会出现，除了在 `exclude` 属性中定义的 `updated_at`：

```
class BlogSerializer(serializers.ModelSerializer):
    class Meta:
        model = Blog
        exclude = ['updated_at']
```

我们应该始终记住，对于 `ModelSerializer`，必须提供 `fields` 或 `exclude`。此外，`exclude` 属性仅映射到 Django 模型类中定义的模型字段。

## `read_only_fields`

单个 `ModelSerializer` 可用于从数据库保存和检索数据。因此，我们会有这样的用例：我们希望某些字段仅在检索数据时序列化，而不在写入操作期间序列化。在此示例中，`updated_at` 字段仅在我们从数据库检索数据时被序列化：

```
class BlogSerializer(serializers.ModelSerializer):
    class Meta:
        model = Blog
        fields = '__all__'
        read_only_fields = ['updated_at']
```

还有另一种定义只读字段的方法——即在定义字段时定义。序列化器字段定义在声明字段定义时接受一个参数 `read_only`。默认情况下，任何 `AutoField` 都设置为只读行为。

## `extra_kwargs`

此属性是 DRF `ModelSerializer` 提供的一个最令人困惑但功能强大的功能之一。由于 DRF 的文档中没有任何细节，因此很难理解如何使用 `extra_kwargs`，但我们将检查一些可使用 `extra_kwargs` 字典配置的基本选项：

* `write_only`：我们可以定义字段是否应为仅写入的布尔类型值
* `min_length`：字段的最小长度，适用于字符串字段
* `max_length`：给定字段的最大长度，适用于字符串字段
* `Required`：用于标记字段是否必需的布尔值
* `Validators`：用于为给定字段设置自定义验证器

除了刚刚提到的之外，还有其他属性，例如 `default` 和 `source`，我们可以根据用例进行配置。让我们看一个如何在模型序列化器中使用 `extra_kwargs` 的基本示例：

```
class BlogSerializer(serializers.ModelSerializer):
    class Meta:
        model = Blog
        fields = '__all__'
        extra_kwargs = {
            'updated_at': {'write_only': True},
            'title': {'min_length': 100, 'required': True},
        }
```

每个父键将是字段名，属性应为键的值——例如，`'updated_at': {'write_only': True}` 表示 `updated_at` 字段为仅写入。

**Important note**

如果字段在序列化器类中明确定义，则 `extra_kwargs` 选项中定义的属性将被始终忽略。

## `Depth`

Django 模型具有外键关系字段和其他相关字段链接。默认情况下，DRF 不会检索除外键之外的任何相关字段值。当我们定义 `depth` 属性时，它会自动遍历关系以检索所有信息，并最终以 JSON 结构表示。以下代码片段将检索所有外键关系值并返回第一级信息：

```
class BlogSerializer(serializers.ModelSerializer):
    class Meta:
        model = Blog
        fields = '__all__'
        depth = 1
```

`depth` 是一个整数值，用于确定序列化器应检索的关系深度，默认情况下 `depth` 设置为 `0`。

现在我们已经了解了如何配置 Model Serializers，让我们学习序列化器的另一个重要方面——关系。

# Implementing Serializer relations

Django 模型有三种类型的关系字段——`ForeignKey`、`ManyToManyField` 和 `OneToOneField`（它们都支持反向关系）。到目前为止，我们已经了解了如何有效地使用模型序列化器，但现在让我们探索如何在序列化器中处理相关字段。

默认情况下，当模型具有任何相关字段时，`ModelSerializer` 会将关联字段链接到序列化器定义中的 `PrimaryKeyRelatedField`。DRF 序列化器将为所有类型的相关字段序列化器使用 `PrimaryKeyRelatedField`，并传递不同的选项。

在这里，我们将学习如何创建相关字段序列化器：

* 对于 `ForeignKey`，DRF 模型序列化器将 `PrimaryKeyRelatedField` 分配给序列化器
* 对于 `OneToOneField`，DRF 模型序列化器将 `PrimaryKeyRelatedField` 分配给序列化器，并附加一个 `UniqueValidator`
* 对于 `ManyToManyField`，DRF 模型序列化器将 `PrimaryKeyRelatedField` 分配给序列化器，并带有两个附加标志 `many=True` 和 `allow_empty=True`

默认情况下，`PrimaryKeyRelatedField` 是读写字段，除非我们特别提及 `read_only=True` 标志。以下是我们如何为相应的相关模型字段使用 `PrimaryKeyRelatedField` 的示例：

```
# blog/models.py
class Blog(models.Model):
    author = models.ForeignKey(
        Author, on_delete=models.PROTECT
    )
    cover_image = models.OneToOneField(
        CoverImage, on_delete=models.PROTECT
    )
    tags = models.ManyToManyField(Tags)
# blog/serializers.py
class BlogCustom3Serializer(serializers.ModelSerializer):
    author = serializers.PrimaryKeyRelatedField(
        queryset=Author.objects.all()
    )
    tags = serializers.PrimaryKeyRelatedField(
        queryset=Tags.objects.all(), many=True, allow_empty=True
    )
    cover_image = serializers.PrimaryKeyRelatedField(
        queryset=CoverImage.objects.all(),      validators=[validators.UniqueValidator(CoverImage.objects.all())]
    )
    class Meta:
        model = Blog
        fields = '__all__'
```

在此示例中，我们手动定义了 DRF 在幕后自动创建的所有不同类型的相关字段序列化器。

`PrimaryKeyRelatedField` 中的 `QuerySet` 参数是什么？

`queryset` 参数用于在我们尝试创建新条目时验证字段输入。对于只读操作，`queryset` 不使用；因此，当我们创建具有只读相关字段的序列化器时，可以忽略它。对于任何写入操作，必须提供 `queryset` 参数，因为它将使用提供的查询集执行验证。

DRF 提供了创建嵌套序列化器的灵活性，这在我们想要获取嵌套对象数据时很有帮助。我们可以使用 `depth` 配置来获取默认字段，但如果我们需要来自嵌套对象的自定义字段，则嵌套序列化器非常方便。

## Working with nested Serializers

我们应该在想要从嵌套对象中获取选定字段时使用嵌套序列化器。例如，让我们为所有博客获取作者姓名和作者简介。`BASerializer` 仅公开作者姓名和简介，因此最终序列化的数据将仅为每个博客的作者姓名和简介。然后，`source` 属性用于告诉序列化器它应该使用哪个数据库模型字段来传递相关对象信息：
