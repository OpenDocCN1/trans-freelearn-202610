

```
<li>{{ book }}</li>
{% endfor %}
</ul>
```

这里，我们使用了 Django 提供的内置 length 过滤器。使用该过滤器会导致对 `book_list` 变量的长度进行求值并返回结果，然后在渲染期间将其插入到我们的 HTML 模板中。

与 length 类似，还有很多其他模板过滤器随 Django 一起预打包并且可供使用。例如，lowercase 过滤器会将文本转换为全小写格式，last 过滤器可用于返回列表中的最后一项，而 json_script 过滤器可用于将传递给模板的 Python 对象作为 JSON 值输出，并在模板中用 `<script>` 标签包裹。

> **重要说明**
> 你可以参考 Django 的官方文档以获取 Django 提供的完整模板过滤器列表，网址为 https://docs.djangoproject.com/en/3.1/ref/templates/builtins/。

现在，在了解了如何使用模板过滤器之后，让我们深入了解如何编写自己的自定义过滤器。

## 自定义模板过滤器

Django 提供了许多有用的过滤器，我们在项目开发过程中可以在模板中使用它们。但如果有人想格式化特定文本并使用不同的字体进行渲染呢？或者说，有人想根据后端中错误代码的映射将错误代码翻译成对用户友好的错误信息。在这些情况下，预定义的过滤器是不够的，我们希望编写一个可以在项目中重复使用的自定义过滤器。

幸运的是，Django 提供了易于使用的 API，我们可以用它来编写自定义过滤器。该 API 为开发人员提供了一些有用的装饰器函数，可用于快速将 Python 函数注册为自定义模板过滤器。一旦 Python 函数被注册为自定义过滤器，开发人员就可以开始在模板中使用该函数。

需要一个模板库方法的实例来访问过滤器。该实例可以通过从 Django 的模板模块实例化 `Library()` 类来创建，如下所示：

```
from django import template
register = template.Library()
```

实例创建后，我们现在可以使用模板库实例中的 filter 装饰器来注册我们的过滤器。

## 创建自定义模板过滤器

创建自定义模板过滤器需要执行几个步骤。让我们尝试了解这些步骤是什么，以及它们如何帮助我们创建自定义模板过滤器，下一个小节将对此进行说明。

### 设置用于存储模板过滤器的目录

需要注意的是，在创建自定义模板过滤器或模板标签时，我们需要将它们放在应用程序目录下的 `templatetags` 目录中。之所以有此要求，是因为 Django 在内部配置为在加载 Web 应用程序时查找自定义模板标签和过滤器。如果目录名称不是 `templatetags`，Django 将不会加载我们创建的自定义模板过滤器和标签。

要创建此目录，首先导航到你想要创建自定义模板过滤器的应用程序文件夹，然后在终端中运行以下命令：

```
mkdir templatetags
```

目录创建后，下一步是在 `templatetags` 目录内创建一个新文件来存储自定义过滤器的代码。这可以通过在 `templatetags` 目录内执行以下命令来完成：

```
touch custom_filter.py
```

> **重要说明**
> 上述命令在 Windows 上不起作用。你可以通过 Windows 资源管理器导航到所需目录并创建新文件。

或者，可以通过 PyCharm 提供的 GUI 界面完成此操作。

### 设置模板库

一旦用于存储自定义过滤器代码的文件创建完成，我们就可以开始着手实现自定义过滤器代码了。为了使自定义过滤器在 Django 中工作，它们需要在模板中使用之前先注册到 Django 的模板库。为此，第一步是设置模板库的一个实例，该实例将用于注册我们的自定义过滤器。为此，在上一节中创建的 `custom_filters.py` 文件内，我们首先需要从 Django 项目导入模板模块：

```
from django import template
```

导入解决后，下一步是创建模板库的实例，添加以下代码行：

```python
register = template.Library()
```

Django 模板模块中的 `Library` 类被实现为一个 **singleton** 类，它返回在应用程序启动时仅初始化一次的相同对象。

模板库实例设置完成后，我们可以继续实现自定义过滤器。

### 实现自定义过滤器函数

Django 中的自定义过滤器不过是简单的 Python 函数，本质上接受以下参数：

- 应用过滤器的值（必需）
- 需要传递给过滤器的任何附加参数（零个或多个）（可选）

这些函数需要用 Django 模板库实例中的 `filter` 属性进行装饰，才能作为模板过滤器发挥作用。例如，自定义过滤器的通用实现如下所示：

```python
@register.filter
def my_filter(value, arg):
    # Implementation logic of the filter
```

至此，我们已经了解了如何实现可在模板中使用的自定义过滤器。在下一节中，我们将学习如何使用这些自定义过滤器。

### 在模板中使用自定义过滤器

过滤器创建后，使用它在模板中非常简单。为此，过滤器必须首先导入到模板中。这可以通过在模板文件顶部添加以下行轻松完成：

```django
{% load custom_filter %}
```

当 Django 的模板引擎解析模板文件时，前一行会自动由 Django 解析，以找到 `templatetags` 目录下指定的正确模块。因此，`custom_filter` 模块中提到的所有过滤器都会自动在模板中可用。

在模板中使用我们的自定义过滤器与添加以下行一样简单：

```django
{{ some_value|generic_filter:"arg" }}
```

掌握了这些知识后，让我们现在创建我们的第一个自定义过滤器。

## 练习 11.01 – 创建自定义模板过滤器

在本练习中，我们将编写一个名为 `explode` 的自定义过滤器，当提供一个字符串和用户提供的分隔符时，它会返回一个字符串列表。例如，考虑以下字符串：

```text
names = "john,doe,mark,swain"
```

你将对该字符串应用以下过滤器：

```django
{{ names|explode:"," }}
```

应用此过滤器后的输出应如下所示：

```text
["john", "doe", "mark", "swain"]
```

让我们开始执行以下步骤：

- 1. 在 `bookr` 项目内创建一个新的应用程序，可用于演示目的：
  
      ```bash
      python manage.py startapp filter_demo
      ```
  
      上述命令将在你的 Django 项目中设置一个新的应用程序。
  
- 2. 现在，在你的 `filter_demo` 应用程序目录内创建一个名为 `templatetags` 的新目录，以存储自定义模板过滤器的代码。要创建目录，请从终端应用程序或命令提示符中的 `filter_demo` 目录内运行以下命令：
  
      ```bash
      mkdir templatetags
      ```
  
- 3. 目录创建后，在 `templatetags` 目录内创建一个名为 `explode_filter.py` 的新文件。
  
- 4. 打开文件并添加以下行：
  
      ```python
      from django import template

      register = template.Library()
      ```
  
      上述代码创建了一个 Django 库的实例，可用于将我们的自定义过滤器与 Django 注册。

