
# Django 管理界面

## Figure 9.6: Django 管理仪表板

点击某个模型会显示现有记录的列表视图。

点击 **Users**，注意通过 shell 添加的用户、作者和文章，以及通过命令行添加的超用户（**STAFF STATUS** 已启用），如 *Figure 9.7* 所示：

![](Images/B34553_09_07.png)

## Figure 9.7: Users

点击 **Posts**，注意现有文章列在一个可筛选的表格中，如 *Figure 9.8* 所示：

![](Images/B34553_09_08.png)

## Figure 9.8: Posts

点击单个博客文章记录会打开一个用于编辑的表单，如 *Figure 9.9* 所示：

![](Images/B34553_09_09.png)

## Figure 9.9: 编辑单个博客文章

理解管理界面的适用范围和局限性很重要。管理界面是为受信任的用户管理内部数据而设计的。它不打算作为面向公众的 UI，你也不应将其视为公共界面。对于编辑、审核员和内部工具，它通常已足够。对于最终用户，通常需要构建自定义视图和模板。

**Prompt**: What are the limits of the Django admin, and when should you stop using it and build custom views?

对于学习而言，管理界面特别有用。它能即时反馈你的模型是否正确、关系是否生效、迁移是否成功运行。许多经验丰富的 Django 开发者在项目上线后仍将管理界面作为诊断工具。

**Prompt**: How does the Django admin interface infer so much functionality from just model definitions?

既然你已了解管理界面及其如何提供自动表单来修改模型数据，接下来让我们看看如何构建自己的表单展示给网站访问者。

# Django 表单

几乎每个交互式网站都需要接受用户输入、验证它，并将其保存到数据库或安全地处理它。Django 的表单系统使用 Python 代码端到端地解决了这个问题，避免了手写分散在视图中的验证逻辑。

从最高层来看，一个 Django 表单由一个 Python 类定义，它做三件事：定义期望的输入、验证该输入、向视图提供清洗后的可信数据。Django 将这些逻辑保持在模板之外，也很大程度上保持在视图之外。

Django 中有两种主要的表单类型，它们派生自内置类：`Form` 和 `ModelForm`。

## 普通表单

基于 `Form` 的类适用于不直接关联数据库模型的输入场景，例如联系表单或搜索框。

一个联系表单的最小示例如下：

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)
```

这个单一的类定义就能给你 HTML 表单字段、验证规则和错误消息。Django 已经知道如何验证邮箱地址、必填字段和最大长度。

## 基于模型的表单

基于 `ModelForm` 的类功能更进一步。它将表单直接关联到模型，并为你处理数据保存。这是 Django 应用中最常见的模式。

例如，一个创建博客文章的表单：

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ["title", "slug", "body", "status", "tags"]
```

Django 会检查模型并自动创建具有正确类型、验证规则和控件的表单字段。如果你稍后修改了模型，表单会自动更新。

在视图中使用表单遵循一个标准模式，你会在各处的 Django 代码库中看到。视图处理两种情况：响应 `GET` 请求显示表单，以及响应 `POST` 请求处理提交的数据（当表单被填写并提交时触发）：

```python
from django.shortcuts import render, redirect
from .forms import PostForm

def create_post(request):
    if request.method == "POST":
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect("post_list")
    else:
        form = PostForm()
    return render(request, "blog/post_form.html", {"form": form})
```

注意上述代码的以下几点：

*   `request.POST` 包含提交的表单数据
*   `is_valid()` 运行所有验证规则并返回布尔值
*   清洗后的验证数据可通过 `form.cleaned_data` 获取
*   `form.save()` 为 `ModelForm` 创建或更新数据库行

如果验证失败，Django 会自动将错误消息附加到表单。你不需要自己编写错误处理逻辑。

**Prompt**: Why does Django separate forms from models instead of just using HTML forms directly?

## 渲染表单

在模板中渲染表单很简单：

```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Save</button>
</form>
```

`{% csrf_token %}` 标签是安全所必需的。Django 默认防范跨站请求伪造，而表单是这种保护最重要的地方。

**Prompt**: Please explain CSRF protection in Django in simple terms with an example attack scenario.

`form.as_p` 适合学习和原型开发。它将每个字段包裹在段落中渲染。实际项目中，通常单独渲染字段以便控制布局和样式：

```html
{{ form.title.label_tag }}
{{ form.title }}
{{ form.title.errors }}
```

Django 表单的一个有用特性是验证发生在视图逻辑继续之前。这意味着你的视图可以假设它们处理的是已清洗和验证的数据。你不必检查邮箱是否有效或必填字段是否缺失。Django 已经完成了这些工作。

**Prompt**: What security problems does Django's forms system solve automatically?

## 表单与其他 Django 组件的集成

表单也与模型、管理界面和模板干净地集成。无论数据来自公共表单还是管理界面，都适用相同的验证规则。

有必要明确表单不是什么。它们不仅仅是 HTML 生成器。它们是位于不可信用户输入和应用逻辑之间的验证和数据清洗层。这种设计是 Django 表单初看稍显繁重，但随着应用增长能节省大量时间和错误的原因。

一旦理解了表单，大量 Django 功能突然变得合理：认证视图、评论系统、管理页面、API 都构建在相同的底层思想之上。

现在让我们看看设置和配置。

# Django 设置与配置

每个 Django 项目由单个配置模块驱动，通常称为 `settings.py`。这个文件回答一个简单的问题：在这个环境中，该项目应如何表现？理解设置不在于记忆选项，而在于知道 Django 去哪里寻找答案。

当你运行 `python manage.py runserver` 时，Django 加载由 `DJANGO_SETTINGS_MODULE` 环境变量定义的设置模块。在默认项目中，这指向 `mysite.settings`。从数据库连接到已安装应用，一切皆由此文件流出。

## 调试模式

你遇到的第一个设置之一是 `DEBUG`：

```python
DEBUG = True
```

当 `DEBUG` 开启时，Django 显示详细错误页面、自动重载代码变更、并为你服务静态文件。这正是学习和开发时所需要的。
