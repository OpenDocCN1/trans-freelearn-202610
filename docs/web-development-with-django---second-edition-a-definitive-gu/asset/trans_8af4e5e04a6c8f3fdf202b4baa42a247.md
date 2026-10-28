
3. 创建此视图后，我们需要将该视图映射到一个 URL 端点，以便可以在浏览器或测试客户端中访问。为此，请打开 `bookr_test` 目录下的 `urls.py` 文件，并向其中添加高亮显示的代码：

```
from django.urls import path
from . import views

urlpatterns = [
    path('test/greet_user', views.greeting_view_user,
        name='greeting_view_user')
]
```

在上面的代码片段中，我们通过设置 `urlpatterns` 列表中的路径，将 `greeting_view` 映射到了应用的 `'test/greet_user'` 端点：

```
path('test/greet_user', views.greeting_view_user,
    name='greeting_view_user')
```

如果你已经完成了之前的练习，这个 URL 应该已经设置好，可用于项目中的检测，无需采取进一步步骤来配置 URL 映射。

4. 视图设置完成后，接下来我们需要验证其是否能正确工作。为此，运行以下命令：

```
python manage.py runserver localhost:8080
```

然后，在您的 Web 浏览器中访问 http://localhost:8080/test/greet_user。

如果您尚未登录，访问上述 URL 时，您将被重定向到项目的登录页面。

5. 现在，为 `greeting_view_user` 编写测试用例，这些用例检查访问 `/test/greet_user` 端点时是否获得成功结果。要实现此测试用例，请打开 `bookr_test` 目录下的 `tests.py` 文件，并向其中添加以下代码：

```
from django.contrib.auth.models import User

class TestLoggedInGreetingView(TestCase):
    """Test the greeting view for the authenticated users."""
    def setUp(self):
        test_user =
            User.objects.create_user(
                username='testuser',
                password='test@#628password')
        test_user.save()
        self.client = Client()

    def test_user_greeting_not_authenticated(self):
```

```
response = self.client.get('/test/greet_user')
self.assertEquals(response.status_code, 302)

def test_user_authenticated(self):
    login = self.client.login(username='testuser',
            password='test@#628password')
    response = self.client.get('/test/greet_user')
    self.assertEquals(response.status_code, 200)
```

在上面的代码片段中，我们实现了一个测试用例，该用例检查那些在内容可见之前需要启用身份验证的视图。

这里，首先我们导入了用于定义测试用例和初始化测试客户端所需的类和方法：

```
from django.test import TestCase, Client
```

接下来，您需要从 Django 的 auth 模块中导入 User 模型：

```
from django.contrib.auth.models import User
```

这个模型是必需的，因为对于需要身份验证的测试用例，我们需要初始化一个新的测试用户。接下来，我们创建了一个名为 `TestLoggedInGreetingView` 的新类，它封装了与 `greeting_user` 视图（该视图需要身份验证）相关的测试。在这个类中，我们定义了三个方法：`setUp()`、`test_user_greeting_not_authenticated()` 和 `test_user_authenticated()`。`setUp()` 方法用于初始化测试用户，该用户将用于身份验证。这是必需的步骤，因为 Django 中的测试环境是一个完全隔离的环境，它不会使用生产应用程序中的数据，因此所有必需的模型和对象都需要在测试环境中单独实例化。

然后，我们使用以下代码创建了测试用户并初始化了测试客户端：

```
test_user = User.objects.create_user(
    username='testuser', password='test@#628password')
test_user.save()
self.client = Client()
```

接下来，我们为用户未通过身份验证时的 `greet_user` 端点编写了测试用例。在这里，您应期望 Django 将用户重定向到登录端点。此重定向可以通过检查响应的 HTTP 状态代码来检测，该状态代码应设置为 HTTP 302，表示重定向操作：

```python
def test_user_greeting_not_authenticated(self):
    response = self.client.get('/test/greet_user')
    self.assertEquals(response.status_code, 302)
```

接下来，我们编写了另一个测试用例，以检查当用户通过身份验证时，`greet_user` 端点是否成功渲染。要对用户进行身份验证，您必须首先调用测试客户端的 `login()` 方法，并通过提供您在 `setUp()` 方法中创建的测试用户的用户名和密码来执行身份验证，如下所示：

```python
login = self.client.login(username='testuser',
                          password='test@#628password')
```

登录完成后，我们必须向 `greet_user` 端点发出 HTTP GET 请求，并通过检查返回响应的 HTTP 状态代码来验证端点是否生成了正确的结果：

```python
response = self.client.get('/test/greet_user')
self.assertEquals(response.status_code, 200)
```

6. 测试用例编写完成后，下面来检查它们如何运行。为此，运行以下命令：

```bash
python manage.py test
```

执行完成后，我们可以预期看到类似如下的响应：

```text
% python manage.py test
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.....
----------------------------------------------------------------------
Ran 5 tests in 0.366s

OK
Destroying test database for alias 'default'...
```

从上面的输出中我们可以看出，我们的测试用例已成功通过，这验证了我们创建的视图能够生成预期的响应：如果用户未通过身份验证，则将用户重定向到网站；如果用户已通过身份验证，则允许用户查看页面。

```
OK
Destroying test database for alias 'default'...
```

从输出中可以看出，我们编写的测试用例已成功通过，从而验证了 RequestFactory 类的行为。

通过本次练习，我们学习了如何利用 RequestFactory 为视图函数编写测试用例，将请求对象直接传递给视图函数，而不是使用测试客户端模拟 URL 访问，从而实现更直接的测试。

在下一节中，我们将测试基于类的视图。

## 测试基于类的视图

在之前的练习中，我们了解了如何测试定义为方法的视图。但基于类的视图又如何？我们应该如何测试它们呢？

事实证明，测试基于类的视图非常容易。例如，如果我们有一个名为 `ExampleClassView(View)` 的基于类的视图，要测试该视图，我们只需使用以下语法：

```
response = ExampleClassView.as_view()(request)
```

就这么简单。

Django 应用程序通常由几个不同的组件组成，有些组件可以单独工作，例如模型，而另一些组件则需要与 URL 映射和框架的其他部分交互才能工作。测试这些不同的组件可能需要一些仅针对这些组件的通用步骤。例如，在测试模型时，我们可能希望在开始测试之前先创建 Model 类的某些对象；或者对于视图，我们可能希望先用用户凭据初始化测试客户端。

事实证明，Django 还基于 TestCase 类提供了其他一些类，这些类可用于针对所使用的组件类型编写特定类型的测试用例。让我们看看 Django 提供的不同类。

## Django 中的测试用例类

除了 Django 提供的基础 TestCase 类（可用于为不同组件定义大量测试用例）之外，Django 还提供了一些从 TestCase 类派生的专门类。

这些类根据它们为开发人员提供的功能，用于特定类型的测试用例。让我们看看 Django 提供的不同类。
