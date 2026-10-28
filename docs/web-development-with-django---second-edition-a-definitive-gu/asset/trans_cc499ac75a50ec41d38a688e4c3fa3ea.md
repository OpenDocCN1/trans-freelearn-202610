

以下是使用 as_ul 渲染的表单：

![](img/4840a43efd335abd10b93708225692d6_320_0.png)

图 6.22：使用 as_ul 渲染的表单

下面是生成的 HTML 示例：

```
<li>
  <label for="id_text_input">Text input:</label>
  <input type="text" name="text_input" required
    id="id_text_input">
</li>
<li>
  <label for="id_password_input">Password input:
  </label>
  <input type="password" name="password_input"
    required id="id_password_input">
</li>
```

+   - as_p

最后，还有 as_p 方法，我们在之前的示例中使用过它。每个输入都包装在 p 标签内，这意味着你不需要像之前的方法那样手动包装表单（在 <table> 或 <ul> 中），：

```
<form method="post">
    {{ form.as_p }}
</form>
```

以下是渲染后的表单的样子：

![](img/4840a43efd335abd10b93708225692d6_321_0.png)

图 6.23：使用 as_p 渲染的表单

你以前见过这个，但再次，这里有一个生成的 HTML 示例：

```
<p>
    <label for="id_text_input">Text input:</label>
    <input type="text" name="text_input" required
        id="id_text_input">
</p>
<p>
    <label for="id_password_input">Password input:
    </label>
    <input type="password" name="password_input"
        required id="id_password_input">
</p>
```

选择哪种方法来渲染你的表单取决于哪种最适合你的应用程序。在行为和在视图中的使用方面，它们都是相同的。在第 15 章中，我们还将介绍一种渲染表单的方法，它将使用 Bootstrap CSS 类。

现在你已经了解了 Django 表单，我们可以更新我们的示例表单页面，使其使用 Django 表单而不是手动编写所有 HTML。我们将在下一个练习中通过用 Django 表单替换手写的 HTML 来实现这一点。

## 练习 6.03 – 构建和渲染一个 Django 表单

在这个练习中，你将使用我们见过的所有字段构建一个 Django 表单。表单和视图的行为将与手动构建的表单类似；然而，你将能够看到使用 Django 编写表单时所需的代码要少得多。你的表单还将自动获得字段验证，并且如果我们对表单进行更改，我们不必再更改 HTML，因为它将根据表单定义动态更新：

- 1. 在 PyCharm 中，在 form_example 应用目录内创建一个名为 forms.py 的新文件。
- 2. 在 forms.py 文件顶部导入 Django forms 库：
- 3. 通过创建 RADIO_CHOICES 变量来定义单选按钮的选项。按如下方式填充它：

```
from django import forms
```

```
RADIO_CHOICES = (
    ("Value One", "Value One Display"),
    ("Value Two", "Text For Value Two"),
    ("Value Three", "Value Three's Display Text")
)
```

你将在创建名为 radio_input 的 ChoiceField 时很快使用它。

保存文件。

6. 打开 form_example 应用的 views.py 文件。在文件顶部，添加一行代码从 forms.py 文件导入 ExampleForm：

```
from .forms import ExampleForm
```

7. 在 form_example 视图内部，实例化 ExampleForm 类并将其分配给 form 变量：

```
form = ExampleForm()
```

8. 使用 form 键将 form 变量添加到上下文字典中。返回行应如下所示：

```
return render(request, "form-example.html",
    {"method": request.method, "form": form})
```

保存文件。

确保你没有删除打印表单发送数据的代码，因为我们将在本练习后面再次使用它。

9. 打开 form_example 应用 templates 目录中的 form-example.html 文件。你可以删除 form 元素的大部分内容，除了 {% csrf_token %} 模板标签和提交按钮。完成后，它应如下所示：

```
<form method="post">
    {% csrf_token %}

    <p>
        <input type="submit" name="submit_input"
               value="Submit Input">
    </p>
    <p>
        <button type="submit" name="button_element"
                value="Button Element">
            Button With <strong>Styled</strong> Text
        </button>
    </p>
</form>
```

10. 使用 as_p 方法添加对 form 变量的渲染。将其放在 {% csrf_token %} 模板标签之后的那一行。整个 form 元素现在应如下所示：

```
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <p>
```

```
```html
<input type="submit" name="submit_input"
       value="Submit Input">
</p>
<p>
    <button type="submit" name="button_element"
            value="Button Element">
        Button With <strong>Styled</strong> Text
    </button>
</p>
</form>
```
```

11. 如果尚未运行，请启动 Django Dev Server，然后在浏览器中访问表单示例页面，地址为 http://127.0.0.1:8000/form-example/。它应如下所示：

![](img/4840a43efd335abd10b93708225692d6_325_0.png)

图 6.24：浏览器中渲染的 Django ExampleForm

## 验证表单并检索 Python 值

到目前为止，我们已经看到 Django 表单如何使使用 Python 代码定义表单变得更加简单，并自动渲染它。现在我们将研究使 Django 表单有用的另一部分：它们能够自动验证表单，然后从中检索原生的 Python 对象和值。

在 Django 中，表单可以是 *未绑定* 或 *已绑定*。这些术语描述表单是否已接收提交的 POST 数据用于验证。到目前为止，我们只看到未绑定的表单——它们是在没有参数的情况下实例化的，像这样：

```
```python
form = ExampleForm()
```
```

如果表单是用一些用于验证的数据调用的，例如 POST 数据，则表单是绑定的。可以这样创建已绑定的表单：

```
```python
form = ExampleForm(request.POST)
```
```

已绑定的表单允许我们开始在表单实例上使用内置的与验证相关的工具。首先是 `is_valid` 方法，它检查表单的有效性，然后是 `cleaned_data` 属性，其中包含从字符串转换为 Python 对象的<|message|>。`cleaned_data` 属性仅在表单已被 *清理* 后可用，清理过程是将数据清理并从字符串转换为 Python 对象的过程。清理过程在 `is_valid` 调用期间运行。如果在调用 `is_valid` 之前尝试访问 `cleaned_data`，将引发 `AttributeError`。

访问 `ExampleForm` 的清理数据的简短示例如下所示：

```
```python
form = ExampleForm(request.POST)

if form.is_valid(): # cleaned_data 只有在表单有效时才会被填充
    if form.cleaned_data["integer_input"] > 5:
        do_something()
```
```

在这个示例中，`form.cleaned_data["integer_input"]` 是整数值 10，因此可以与数字 5 进行比较。将其与已发布的值进行比较，后者是字符串 10。清理过程为我们执行了此转换。其他字段如日期或布尔值也会相应转换。

清理过程还会在表单和字段上设置任何错误，这些错误将在再次渲染表单时显示。让我们看看这一切的实际效果。现代浏览器提供大量客户端验证，因此它们会防止表单在未满足基本验证规则时被提交。如果你尝试在上一个练习中提交表单且字段为空，你可能已经看到过这种情况：

![](img/4840a43efd335abd10b93708225692d6_328_0.png)

*图 6.26* 显示浏览器阻止表单提交。由于浏览器阻止了提交，Django 从未有机会自行验证表单。为了允许表单被提交，我们需要添加一些浏览器无法自行验证的更高级验证。我们将在下一节讨论可以应用于表单字段的不同类型的验证，但现在，我们只需将 `max_digits` 设置为 3 添加到 `ExampleForm` 的 `decimal_input`。这意味着用户不应在表单中输入超过三位数。

> **注意**
> 
> 为什么 Django 应该验证表单如果浏览器已经在这样做并阻止提交？服务器端应用程序永远不应信任来自用户的输入：用户可能使用旧浏览器或其他 HTTP 客户端发送请求，因此不会从浏览器收到任何错误。此外，正如我们刚才提到的，有一些浏览器不理解的验证类型，因此 Django 必须在自己的端验证这些。

