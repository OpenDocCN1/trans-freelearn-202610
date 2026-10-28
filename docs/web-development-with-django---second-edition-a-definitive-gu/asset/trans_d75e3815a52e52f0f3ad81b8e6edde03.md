
```
```python
...

from django.core.exceptions import PermissionDenied
from PIL import Image
from django.contrib import messages

...

@login_required
def review_edit(request, book_pk, review_pk=None):
    book = get_object_or_404(Book, pk=book_pk)

    if review_pk is not None:
        review = get_object_or_404(Review,
                                   book_id=book_pk,
                                   pk=review_pk)
        user = request.user
        if not user.is_staff and review.creator.id != user.id:
            raise PermissionDenied
    else:
        review = None

...
```
```

现在，当非工作人员用户尝试编辑其他用户的评论时，将抛出 Forbidden 错误，如图 *9.7* 所示。在下一节中，我们将研究如何在模板中应用条件逻辑，这样用户就不会进入其权限不足而无法访问的页面：

![](img/4840a43efd335abd10b93708225692d6_471_0.png)

图 9.7 – 非工作人员用户禁止访问

在本练习中，我们使用身份验证装饰器来保护 Django 应用中的视图。所应用的这些身份验证装饰器提供了一种简单的机制，来限制缺少必要权限的用户、非工作人员用户以及未认证用户访问视图。Django 的身份验证装饰器提供了一种遵循 Django 角色和权限框架的健壮机制，而 `user_passes_test` 装饰器则提供了开发自定义身份验证的选项。

现在我们已经使用装饰器控制了应用的身份验证流程，接下来可以利用用户身份验证数据来自定义模板。

## 使用身份验证数据增强模板

在练习 9.02——添加个人资料页面中，我们看到可以将 `request.user` 对象传递给模板，以在 HTML 中渲染当前用户的属性。我们也可以根据用户类型或用户所拥有的权限，采用不同的模板渲染方式。考虑我们要添加一个仅对工作人员用户可见的编辑链接，可以使用一个 if 条件来实现这一点：

```
{% if user.is_staff %}
<p><a href="{% url 'review:edit' %}">Edit this Review</a>
</p>
{% endif %}
```

如果我们不花时间根据权限有条件地渲染链接，用户在应用中导航时会感到沮丧，因为他们点击的许多链接都会导致 403 禁止访问页面。接下来的练习将展示如何利用模板和身份验证在项目中呈现与上下文相符的链接。

## 练习 9.04 ——在基础模板中切换登录和退出登录链接

在 bookr 项目的基础模板中（位于 `templates/base.html`），我们在页头有一个占位符形式的退出登录链接。其 HTML 代码如下：

```
<li class="nav-item">
<a class="nav-link" href="#">Logout</a>
</li>
```

我们不希望用户在退出登录后仍然看到退出登录链接。因此，本练习旨在对模板应用条件逻辑，以便根据用户是否已认证来切换登录和退出登录链接：

- 1. 编辑 `templates/base.html` 文件。复制退出登录列表元素的结构并创建一个登录列表元素。然后，将占位符链接替换为退出登录和登录页面的正确 URL——分别为 `/accounts/logout` 和 `/accounts/login`，如下所示：

```
```html
<li class="nav-item">
    <a class="nav-link" href="/accounts/logout">Logout
    </a>
</li>
<li class="nav-item">
    <a class="nav-link" href="/accounts/login">Login</a>
</li>
```
```

- 2. 现在，将我们的两个 `li` 元素放在一个 `if ... else ... endif` 条件块中。我们要应用的逻辑条件是 `user.is_authenticated`：

```
```django
{% if user.is_authenticated %}
    <li class="nav-item">
        <a class="nav-link" href="/accounts/logout">Logout
        </a>
    </li>
{% else %}
    <li class="nav-item">
        <a class="nav-link" href="/accounts/login">Login
        </a>
    </li>
{% endif %}
```
```

- 3. 现在，访问用户个人资料页面 `http://localhost:8000/accounts/profile/`。当已认证时，你会看到退出登录链接：

![](img/4840a43efd335abd10b93708225692d6_473_0.png)

图 9.8 – 已认证用户会看到退出登录链接

4. 现在，点击退出登录链接；你将被带到 `/accounts/logout` 页面。菜单中会出现登录链接，这证实了该链接是根据用户的认证状态而上下文相关的：

![](img/4840a43efd335abd10b93708225692d6_474_0.png)

图 9.9 – 未认证用户会看到登录链接

本练习是 Django 模板如何与身份验证信息结合使用，以创建有状态且上下文相关的用户体验的一个简单示例。我们也不希望提供用户无权访问的链接，或用户权限级别不允许执行的操作。以下活动将使用这一模板技术来修复 Bookr 中的一些此类问题。

## 活动 9.01 ——在模板中使用条件块生成基于身份验证的内容

在本活动中，你将在模板中应用条件块，根据用户认证和用户状态修改内容。不应向用户展示他们无权访问的链接或未被授权执行的操作。以下步骤将帮助你完成本活动：

- 1. 在 `book_detail` 模板中（文件位于 `reviews/templates/reviews/book_detail.html`），对未认证用户隐藏“添加评论”和“媒体”按钮。
- 2. 另外，隐藏显示“成为第一个撰写评论的人”的标题，因为这不是未认证用户可用的选项。
- 3. 在同一个模板中，让“编辑评论”链接仅对工作人员用户或撰写该评论的用户显示。该模板块的条件逻辑与我们在上一节的 `review_edit` 视图中使用的条件逻辑非常相似：

![](img/4840a43efd335abd10b93708225692d6_475_0.png)

图 9.10 – 当 Alice 登录后，她的评论上会出现编辑评论链接

当另一个用户登录时，**编辑评论**链接将不会出现：

![](img/4840a43efd335abd10b93708225692d6_475_1.png)

图 9.11 – 当 Bob 登录时，Alice 的评论上没有编辑评论链接

- 4. 修改 `template/base.html`，使其在页头搜索表单右侧显示当前已认证用户的用户名，并链接到用户个人资料页面。

完成本活动后，你将在模板中添加反映当前用户认证状态和身份的动态内容，如下面的截图所示：

![](img/4840a43efd335abd10b93708225692d6_475_2.png)

图 9.12 – 已认证用户的名称出现在搜索表单之后

> **注意**
> 你可以在此处找到本活动的解决方案：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions。

至此，我们已经回顾了 Django 中可用的身份验证机制，接下来可以考察 Django 如何实现会话。

## 会话

了解一些理论有助于理解为什么会话是 Web 应用中管理用户内容的常见解决方案。HTTP 协议定义了客户端与服务器之间的交互。它被称为“无状态”协议，因为服务器在请求之间不保留任何状态信息。这种协议设计在万维网早期非常适用于传递超文本信息，但并不适合那些向特定用户传递定制信息的安全 Web 应用的需求。

我们现在已经习惯看到网站适应我们的浏览习惯。购物网站会推荐与我们最近浏览过的商品相似的商品，并告知我们本地区热门的商品。这些功能都需要一种有状态的网站开发方法。实现有状态 Web 体验的最常见方式之一就是通过会话。会话是指用户当前与 Web 服务器或应用的交互，要求在交互期间持续保存数据。这可能包括用户访问过的链接、执行过的操作，以及在交互中做出的偏好设置等信息。

如果用户在一个页面上将博客网站设置为深色主题，那么人们会期望下一页也使用相同的主题。我们将这种表现描述为维持状态。会话密钥作为浏览器 cookie 存储在客户端，可以与用户登录期间持续存在的服务器端信息进行关联。

在 Django 中，会话以中间件的形式实现。当我们最初在第 4 章《Django Admin 简介》中创建应用时，会话支持已经默认激活。

### 会话引擎

当前和已过期会话的信息需要存储在某个地方。在万维网早期，这是通过将会话信息保存在服务器上的文件中完成的，但随着 Web 服务器架构变得更加复杂、性能需求不断提高，诸如数据库或内存存储等更高效的策略已成为常态。默认情况下，在 Django 中，会话信息存储在项目的数据库中。

对于大多数小型项目来说，这是一个合理的默认设置。然而，Django 的中间件会话实现使我们能够灵活地以多种方式存储项目的会话信息，以

