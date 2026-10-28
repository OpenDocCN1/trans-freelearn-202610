

## 练习 9.01 – 改造 Admin 应用的登录模板

我们在本章开始时没有为项目提供登录页面。通过添加用于身份验证的 URL 模式，并将 Admin 应用中的模板复制到项目中，我们可以实现登录页面的功能。但这个登录页面直接从 Admin 应用复制而来，与 Bookr 的设计脱节，因此并不令人满意。在本练习中，我们将按照所需的步骤，将 Admin 应用的登录模板改造为适用于我们项目的模板。新的登录模板需要直接从 bookr 项目的 `templates/base.html` 中继承样式和格式：

1. 在项目中创建一个 `templates/registration` 目录。

Admin 登录模板位于 Django 源码目录下的 `django/contrib/admin/templates/admin/login.html` 路径。它以 `extends` 标签、`load` 标签开始，导入 `i18n` 和 `static` 模块，并包含一系列覆盖父模板 `django/contrib/admin/templates/admin/base.html` 中定义块的 `block` 扩展。`login.html` 文件的截断片段如以下代码块所示：

```
{% extends "admin/base_site.html" %}
{% load i18n static %}

{% block extrastyle %}{{ block.super }}...
{% endblock %}

{% block bodyclass %}{{ block.super }} login{% endblock %}
{% block usertools %}{% endblock %}
{% block nav-global %}{% endblock %}
{% block content_title %}{% endblock %}
{% block breadcrumbs %}{% endblock %}
```

2. 将此 Admin 登录模板 `django/contrib/admin/templates/admin/login.html` 复制到 `templates/registration` 中，并使用 PyCharm 开始编辑该文件。

3. 由于你正在编辑的登录模板位于 `templates/registration/login.html`，并扩展了基础模板（`templates/base.html`），请将 `templates/registration/login.html` 顶部的 `extends` 标签参数替换为以下内容：

```
{% extends "base.html" %}
```

4. 我们不需要这个文件的大部分内容。只需保留包含登录表单的 `content` 块。模板的其余部分将包含加载 `i18n` 和 `static` 标签库的内容：

```
{% load i18n static %}

{% block content %}
...
{% endblock %}
```

5. 现在，你必须将 `templates/registration/login.html` 中的路径和反向 URL 模式替换为适合你的项目的路径和反向 URL 模式。由于你的模板中没有定义 `app_path` 变量，因此需要用登录的反向 URL `'accounts:login'` 来替换它。所以，考虑下面这一行：

```
<form action="{{ app_path }}" method="post" id="login-form">
```

这一行将更改为：

```
<form action="{% url 'accounts:login' %}" method="post"
id="login-form">
```

你的项目路径中没有定义 `'admin_password_reset'`，因此它将替换为 `'accounts:password_reset'`。

考虑下面这一行：

```
{% url 'admin_password_reset' as password_reset_url %}
```

这将更改为以下一行：

```
{% url 'accounts:password_reset' as password_reset_url %}
```

你的登录模板将如下所示：

`templates/registration/login.html`

```
{% extends "base.html" %}
{% load i18n static %}

{% block content %}
{% if form.errors and not form.non_field_errors %}
<p class="errornote">
{% if form.errors.items|length == 1 %}{% translate "Please correct the error below." %}{% else %}{% translate "Please correct the errors below." %}{% endif %}
</p>
{% endif %}
```

你可以在以下网址找到该文件的完整代码：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter09/Exercise9.01/bookr。

6. 如前所述，要使用标准的 Django 身份验证视图，我们必须为它们添加 URL 映射。打开 bookr 项目目录中的 `urls.py` 文件，确保从 `django.contrib` 中导入了 `auth`，并将这三个路径添加到 URL 模式中：

```
from django.contrib import admin, auth
...
urlpatterns = [
    ...
    path("accounts/",
        include(("django.contrib.auth.urls", "auth"),
        namespace="accounts")),
    path("accounts/password_reset/done/",
        auth.views.PasswordResetDoneView.as_view(),
        name="password_reset_done"),
    path("accounts/reset/done/",
        auth.views.PasswordResetCompleteView.as_view(),
        name="password_reset_complete"),
    ...
]
```

7. 现在，当你访问 http://127.0.0.1:8000/accounts/login/ 处的登录链接时，你将看到此页面：

![](img/4840a43efd335abd10b93708225692d6_462_0.png)

图 9.3 – Bookr 登录页面

完成此练习后，你已在项目中创建了非管理员身份验证所需的模板。

> **注意**
>
> 在继续之前，你需要确保 `registration` 目录中的其余模板遵循 bookr 项目的样式；也就是说，它们不能继承自 Admin 应用的 `admin/base_site.html` 模板。你已经看到了 `password_change_done.html` 和 `login.html` 模板是如何做到这一点的。请将你在本练习（以及之前部分）中学到的知识应用到 `registration` 目录中的其余文件。或者，你可以从本书的 GitHub 仓库下载修改后的文件：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter09/Activity9.01/bookr。

现在我们已经修改了项目以启用身份验证，接下来我们将学习 Django 中的密码和会话存储。

## Django 中的密码存储

Django 不会将密码以明文形式存储在数据库中。相反，密码会使用 PBKDF2/SHA256、BCrypt/SHA256 或 Argon2 等哈希算法进行摘要处理。由于哈希算法是单向转换，这可以防止用户的密码从数据库中存储的哈希值被解密。用户通常期望系统管理员能找回他们遗忘的密码，因此这常常令他们感到意外，但这是安全设计中的最佳实践。所以，如果我们查询数据库中的密码，会看到类似这样的内容：

```
sqlite> select password from auth_user;
pbkdf2_sha256$320000$auohDtokS8a6LAoaG79Tv1$Mxzssu6gfd5uZvI1ADFI4faAjP0g3puP2JxxFdWZI0=
pbkdf2_sha256$320000$SqoVF8nhgo4zgw4juRUNtAK5TATQEqXtbRAfc4Ccuuwr9nRHVuShZo5W6N90ygpvVbA=
pbkdf2_sha256$320000$Ss3eprgniu7e4q0ux9Xj5nh$/SAFKgXuYIWSm08PWEwBUsMfmPCYd60rAaz0JZnLKGs=
```

图 9.4 – Django SQLite3 数据库中的密码哈希

此字符串的组成部分为 `<algorithm>$<iterations>$<salt>$<hash>`。由于一些哈希算法会随着时间推移而被攻破，而且我们有时需要满足强制性的安全要求，Django 足够灵活，能够容纳新的算法，并可以维护用多种算法加密的数据。

## 个人资料页面与 `request.user` 对象

当登录成功时，登录视图会重定向到 `/accounts/profile`。但是，这个路径并未包含在现有的身份验证 URL 中，身份验证应用也没有为其提供模板。为了避免“Page not found”错误，需要一个视图和相应的 URL 模式。

每个 Django 请求都有一个 `request.user` 对象。如果请求是由未认证用户发出的，`request.user` 将是一个 `AnonymousUser` 对象。如果请求是由已认证用户发出的，那么 `request.user` 将是一个 `User` 对象。这使得在 Django 视图中检索个性化用户信息并在模板中渲染变得很容易。

在下一个练习中，我们将向 bookr 项目添加一个个人资料页面。

## 练习 9.02 – 添加个人资料页面

在本练习中，我们将向项目添加一个个人资料页面。为此，我们需要在 URL 模式中包含其路径，并在视图和模板中同时包含它。个人资料页面将简单地显示 `request.user` 对象中的以下属性：

- `username`
- `first_name` 和 `last_name`
- `date_joined`
- `email`
- `last_login`

执行以下步骤以完成本练习：

1. 将 `bookr/views.py` 添加到项目中。它需要一个简单的 `profile` 函数来定义我们的视图：

```
from django.shortcuts import render

def profile(request):
    return render(request, 'profile.html')
```

2. 在你的主 bookr 项目的 `templates` 文件夹中，创建一个名为 `profile.html` 的新文件。在此模板中，可以使用类似 `{{ request.user.username }}` 的表示法轻松引用 `request.user` 对象的属性：

```
{% extends "base.html" %}

{% block title %}Bookr{% endblock %}

{% block content %}
<h2>Profile</h2>
<div>
    <p>
        Username: {{ request.user.username }} <br>
        Name: {{ request.user.first_name }} {{
```

