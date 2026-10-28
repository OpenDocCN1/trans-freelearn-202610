

完成此操作后，我们现在可以使用以下命令创建一个新文件来存储自定义模板标签的代码：

```
touch custom_tags.py
```

> **重要提示**
> 
> 上述命令在 Windows 上无法使用。 但是，您可以使用 Windows 资源管理器创建一个新文件。

## 设置模板库

设置好目录结构并为存储自定义模板标签代码的文件做好准备后，我们现在可以开始创建模板标签。 但在此之前，我们需要像之前一样设置 Django 模板库的实例。 这可以通过向我们的 custom_tag.py 文件添加以下代码行来完成：

```
from django import template
register = template.Library()
```

与自定义过滤器一样，这里使用的模板库实例用于在 Django 模板中注册自定义模板标签以供使用。

## 实现一个简单的模板标签

Django 中的简单模板标签是可以接受我们想要的任意数量参数的 Python 函数。 这些 Python 函数需要用模板库中的 simple_tag 装饰器进行装饰，以将这些函数注册为简单模板标签。 以下代码片段展示了简单模板标签是如何实现的：

```
@register.simple_tag
def generic_simple_tag(arg1, arg2):
    # Logic to implement a generic simple tag
```

接下来，让我们将这些简单标签用于模板中。

## 在模板中使用简单标签

在 Django 模板中使用简单标签非常容易。 在模板文件中，我们首先需要确保标签已在模板中导入，通过将以下内容添加到模板文件的顶部：

```
{% load custom_tag %}
```

前面的语句将从我们之前定义的 `custom_tag.py` 文件加载所有标签，并使它们在模板中可用。 然后我们可以通过添加以下命令来使用我们的自定义简单标签：

```
```django
{% custom_simple_tag "argument1" "argument2" %}
```
```

不是很容易吗？！ 现在，让我们将这些知识付诸实践并创建我们的第一个自定义简单标签。

## 练习 11.02 – 创建一个自定义简单标签

在本练习中，我们将创建一个简单标签，它将接受两个参数：第一个是问候信息，第二个是用户的姓名。 该标签将打印格式化的问候信息。 让我们开始步骤：

- 1. 延续 *练习 11.01* 中显示的示例，让我们重用相同的目录结构来存储简单标签的代码。 因此，首先在 `filter_demo` 目录下创建一个名为 `simple_tag.py` 的新文件。 在此文件中，添加以下代码：

```
```python
from django import template

register = template.Library()

@register.simple_tag
def greet_user(message, username):
    return "{greeting_message}, {user}!!!"
        .format(greeting_message=message, user=username)
```
```

在这种情况下，您创建一个新的 Python 方法 `greet_user()`，它接受两个参数，`message`，用于问候的消息，以及 `username`，应被问候的用户的姓名。 然后该方法用 `@register.simple_tag` 装饰，表示该方法是一个简单标签，可以用作模板中的模板标签。

- 2. 现在，创建一个将使用您的简单标签的新模板。 为此，在 `filter_demo/templates` 目录下创建一个名为 `simple_tag_template.html` 的新文件，并向其中添加以下代码：

```
```html
<html>
<head>
    <title>Simple Tag Template Example</title>
</head>
<body>
{% load simple_tag %}
{% greet_user "Hey" username %}
</body>
</html>
```
```

在浏览器中访问 http://localhost:8000/filter_demo/greet 后，您应该会看到以下页面：

![](img/4840a43efd335abd10b93708225692d6_531_0.png)

图 11.2 – 使用自定义简单标签生成的问候信息

通过此操作，我们创建了第一个自定义模板标签，并成功地使用它来渲染模板，如图 11.2 所示。 现在，让我们看看简单标签的另一个重要方面，即将模板中可用的上下文变量传递给模板标签。

## 在自定义模板标签中传递模板上下文

在之前的练习中，我们创建了一个简单标签，我们向其传递了两个参数，问候信息和用户名。 但是如果我们想向标签传递大量变量呢？ 或者简单地说，如果我们不想显式地将用户的用户名传递给标签怎么办？

有时开发人员希望模板中存在的所有变量和数据都能在自定义标签内部使用。 幸运的是，对我们来说，实现起来很容易。

使用我们之前 `greet_user` 标签的示例，让我们创建一个名为 contextual_greet_user 的新标签，看看我们如何将模板中可用的数据直接传递给标签，而不是手动将其作为参数传递。

我们需要做的第一个修改是将我们的装饰器修改为如下所示：

```
@register.simple_tag(takes_context=True)
```

通过此操作，我们告诉 Django，当使用我们的 contextual_greet_user 标签时，Django 还应将模板上下文传递给它，该上下文包含从视图传递到模板的所有数据。 完成此添加后，我们需要做的下一件事是更改我们的 contextual_greet_user 实现以接受作为参数添加的上下文。 以下代码显示了使用模板上下文来渲染问候信息的 contextual_greet_user 标签的修改形式：

```
@register.simple_tag(takes_context=True)
def contextual_greet_user(context, message):
    username = context['username']
    return "{greeting_message}, {user}"
        .format(greeting_message=message, user=username)
```

在前面的代码示例中，我们可以看到 contextual_greet_user() 方法是如何被修改为接受作为第一个参数传递的上下文，然后是用户传递的问候信息。

为了利用此修改后的模板标签，我们需要做的就是将 simple_tag_template.html 下 filter_demo 中对 contextual_greet_user 标签的调用更改为如下所示：

```
{% contextual_greet_user "Hey" %}
```

然后，当我们重新加载 Django Web 应用程序时，http://localhost:8000/filter_demo/greet 处的输出应与练习 11.02 – 创建自定义简单标签的步骤 5 中显示的内容相同。

通过此操作，我们学习了如何构建简单标签并处理将模板上下文传递给标签。 现在，让我们看看如何构建一个包含标签，该标签可用于以另一个模板描述的格式呈现数据。

## 包含标签

简单标签允许我们构建接受一个或多个输入变量、对它们进行一些处理并返回输出的标签。 然后此输出将插入简单标签使用的位置。

但是如果我们想构建标签，而不是返回文本输出，而是返回 HTML 模板，该模板可用于呈现页面的各个部分怎么办？ 例如，许多 Web 应用程序允许用户向其个人资料添加自定义小部件。 这些单独的小部件可以构建为包含标签并独立呈现。 这种方法使基本页面模板和单独模板的代码保持分离，从而便于重用和重构。

开发自定义包含标签的过程与开发简单标签的过程类似。 这涉及使用模板库提供的 inclusion_tag 装饰器。 因此，让我们看看我们是如何做到的。

## 练习 11.03 – 构建自定义包含标签

在本练习中，我们将构建一个自定义包含标签，它将呈现用户阅读的书籍列表：

- 1. 对于本练习，您将继续使用与早期练习中相同的演示文件夹。 首先，在 filter_demo/templates 目录下创建一个名为 inclusion_tag.py 的新文件，并在其中写入以下代码：

```
from django import template

register = template.Library()

@register.inclusion_tag('book_list.html')
def book_list(books):
    book_list = [book_name for book_name, book_author
                 in books.items()]
    return {'book_list': book_list}
```

@register.inclusion_tag 装饰器用于将方法标记为自定义包含标签。 此装饰器采用应用于呈现标签函数返回的数据的模板的名称作为参数。

在装饰器之后，我们定义了一个函数来实现自定义包含标签的逻辑。 此函数接受一个名为 books 的单个参数。 此参数将从模板文件传递，并包含读者阅读的书籍列表（以 Python 字典的形式）。 在定义中，我们将字典转换为 Python 书名列表。 字典中的键映射到书名，值映射到作者：

```
books_list = [book_name for book_name, book_author in books.items()]
```

形成列表后，以下代码将列表作为上下文返回给传递给包含标签的模板（在本例中为 book_list.html）：

```
return {'book_list': books_list}
```

此方法返回的值将由 Django 传递给 book_list.html 模板，然后内容将被呈现。

- 2. 接下来，创建实际的模板，该模板将包含模板标签的呈现结构。 为此，在 filter_demo/templates 目录下创建一个新的模板文件 book_list.html，并向其中添加以下内容：

```
<ul>
    {% for book in book_list %}
    <li>{{ book }}</li>
    {% endfor %}
</ul>
```

