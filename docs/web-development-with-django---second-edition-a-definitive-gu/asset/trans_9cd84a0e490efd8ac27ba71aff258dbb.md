

```
{user.email} ") self.stdout.write(pformat(data))
```

11. 现在，你可以使用这个管理命令来检查存储在数据库中的会话数据。你可以在命令行中按如下方式调用它：

```
python manage.py sessioninfo
```

这将有助于在尝试最终活动时调试会话行为：

```
> python manage.py sessioninfo
Session Key: ocgo7sypaz01tlw92dr1ffkrogy0ux03 User: 2 carol carol.brown@example.com
{'_auth_user_backend': 'django.contrib.auth.backends.ModelBackend',
'_auth_user_hash': 'eb5de6bf54460bafc9e0f555c65d16b17b3fdf754617d1d73529d9605fea2f0c',
'_auth_user_id': '2'}
Session Key: y2r3d7fwtpmsmrn2azcha2a01ehdnbhr User: 1 bob bob.black@example.com
{'_auth_user_backend': 'django.contrib.auth.backends.ModelBackend',
'_auth_user_hash': '322625efba7788f598d02fca5ad86bb14db4893f3c92bc4099822ea3bf4d1e4f',
'_auth_user_id': '1'}
Session Key: gyfciciqm8ht8vmx3c8i8m8m6zh1ltgl User: 3 bookradmin bookradmin@example.com
{'_auth_user_backend': 'django.contrib.auth.backends.ModelBackend',
'_auth_user_hash': 'a30d9315c9b5dda38802da5e8e64f94b0b063f0136fad8456205ed8cced8ff30',
'_auth_user_id': '3'}
```

图 9.16 – Python 脚本

此脚本输出解码后的会话信息。目前，会话仅包含三个键：

- `_auth_user_backend` 是用户后端类的字符串表示。由于我们的项目将用户凭据存储在模型中，因此使用 ModelBackend。

- `_auth_user_hash` 是用户密码的哈希值。

- `_auth_user_id` 是从模型的 `User.id` 属性获得的用户 ID。

此练习帮助你熟悉 Django 中会话数据的存储方式。现在，我们将把注意力转向向 Django 会话中添加额外信息。

## 在会话中存储数据

我们已经介绍了 Django 中会话的实现方式。现在，我们将简要研究一些可以利用会话来丰富用户体验的方法。在 Django 中，会话是请求对象的属性。它被实现为类似字典的对象。在我们的视图中，我们可以像典型的字典一样为会话对象分配键，如下所示：

```
request.session['books_reviewed_count'] = 39
```

但有一些限制。首先，会话中的键必须是字符串，因此不允许使用整数和时间戳。其次，以下划线开头的键保留供内部系统使用。数据仅限于可以编码为 JSON 的值，因此某些无法解码为 UTF-8 的字节序列，例如之前列出的 `binary_key`，不能存储为 JSON 数据。另一个警告是避免将 `request.session` 重新分配给不同的值。我们应该只分配或删除键。因此，不要这样做：

```
request.session = {'books_read_count':30, 'books_reviewed_count': 39}
```

而是这样做：

```
request.session['books_read_count'] = 30
request.session['books_reviewed_count'] = 39
```

在了解这些限制后，我们将研究如何在 Reviews 应用中使用会话数据。

### 练习 9.06 – 在会话中存储最近查看的书籍

本练习的目的是使用会话来保存经过身份验证的用户最近浏览的 10 本书的信息。此信息将显示在 bookr 项目的个人资料页面上。当浏览一本书时，会调用 `book_detail` 视图。在本练习中，我们将编辑 `reviews/views.py` 并向 `book_detail` 方法添加一些额外逻辑。我们将在会话中添加一个名为 `viewed_books` 的键。利用 HTML 和 CSS 的基本知识，可以创建页面以在页面的不同分区中显示个人资料详细信息和已查看的书籍，如下所示：

![](img/4840a43efd335abd10b93708225692d6_484_0.png)

图 9.17 – 包含已查看书籍的个人资料页面

让我们开始步骤：

1. 编辑 `reviews/views.py` 和 `book_detail` 方法。我们只对经过身份验证的用户添加会话信息感兴趣，因此添加一个条件语句来检查用户是否已通过身份验证，并将 `max_viewed_books_length`，即已查看书籍列表的最大长度，设置为 10：

```python
def book_detail(request, pk):
    ...
    if request.user.is_authenticated:
        max_viewed_books_length = 10
```

2. 在同一个条件块内，添加代码以检索 `request.session['viewed_books']` 的当前值。如果会话中不存在此键，则从空列表开始：

```python
viewed_books = request.session.get(
    'viewed_books', [])
```

3. 如果当前书籍的主键已存在于 `viewed_books` 中，则以下代码将删除它：

```python
viewed_book = [book.id, book.title]
if viewed_book in viewed_books:
    viewed_books.pop(
        viewed_books.index(viewed_book))
```

4. 以下代码将当前书籍的主键插入到 `viewed_books` 列表的开头：

```python
viewed_books.insert(0, viewed_book)
```

5. 添加以下键以仅保留列表的前 10 个元素：

```python
viewed_books = viewed_books[
    :max_viewed_books_length]
```

6. 以下代码将我们的 `viewed_books` 添加回 `request.session['viewed_books']`，以便在后续请求中可用：

```python
request.session['viewed_books'] = viewed_books
```

7. 如前所述，在 `book_detail` 函数的末尾，使用请求和上下文数据渲染 `reviews/book_detail.html` 模板：

```python
return render(request, "reviews/book_detail.html", context)
```

完成后，`book_detail` 视图将具有以下条件块：

```python
def book_detail(request, pk):
    ...
    if request.user.is_authenticated:
        max_viewed_books_length = 10
        viewed_books = request.session.get("viewed_books", [])
        viewed_book = [book.id, book.title]
        if viewed_book in viewed_books:
            viewed_books.pop(viewed_books.index(viewed_book))
        viewed_books.insert(0, viewed_book)
        viewed_books = viewed_books[:max_viewed_books_length]
        request.session["viewed_books"] = viewed_books
    return render(request, "reviews/book_detail.html", context)
```

8. 修改 `templates/profile.html` 的页面布局和 CSS 以容纳已查看书籍分区。由于我们将来可能在此页面中添加更多分区，因此一个方便的布局概念是 **flexbox**。我们将添加此 CSS 并将内容分离为嵌套的 div 实例，这些实例将在页面上水平排列。我们将内部 div 实例称为 `infocell` 实例，并为其设置绿色边框和圆角样式：

```
<style>
.flexrow { display: flex;
           border: 2px black;
}
.flexrow > div { flex: 1; }

.infocell {
    border: 2px solid green;
    border-radius: 5px 25px;
    background-color: white;
    padding: 5px;
    margin: 20px 5px 5px 5px;
}

</style>

<div class="flexrow" >
    <div class="infocell" >
        <p>Profile</p>
        ...
    </div>

    <div class="infocell" >
        <p>Viewed Books</p>
        ...
    </div>
</div>
```

9. 修改 `templates/profile.html` 中的已查看书籍 div 元素，以便如果存在书籍，则显示其标题并链接到各个书籍详情页。这将呈现如下：

```
<a href="/books/1">Advanced Deep Learning with Keras</a><br>
```

如果列表为空，则应显示一条消息。整个 div，包括对 `request.session.viewed_books` 的迭代，将如下所示：

```
<div class="infocell" >
    <p>Viewed Books</p>
    <p>
    {% for book_id, book_title in
    request.session.viewed_books %}
    <a href="/books/{{ book_id }}">{{ book_title }}</a><br>
    {% empty %}
        No recently viewed books found.
    {% endfor %}
    </p>
</div>
```

一旦合并所有这些更改，完整的个人资料模板将如下所示：

### templates/profile.html

```
{% extends "base.html" %}

{% block title %}Bookr{% endblock %}

{% block heading %}Profile{% endblock %}

{% block content %}

<style>
```

你可以在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter09/Exercise9.06/bookr/templates/profile.html 找到此文件的完整代码。
