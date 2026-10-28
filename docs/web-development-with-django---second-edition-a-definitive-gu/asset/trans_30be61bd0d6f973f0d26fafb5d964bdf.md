

一旦命令执行完毕，你将看到与此处所示类似的输出：

```
% python manage.py test
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
..
----------------------------------------------------------------------
Ran 2 tests in 0.002s

OK
Destroying test database for alias 'default'...
```

正如前述输出所示，测试用例已成功执行，从而验证了诸如创建新的 Publisher 对象以及在获取时对其字符串表示的操作等，都是正确进行的。

通过本练习，我们看到了如何轻松地为 Django 模型编写测试用例，并通过创建对象、检索它们并对其进行表示来验证其功能。

此外，请注意练习 14.02 中测试 Django 模型的输出中的以下重要行：

```
"Destroying test database for alias 'default'..."
```

这是因为当存在需要将数据持久化到数据库中的测试用例时，Django 不会使用生产数据库，而是为测试用例创建一个新的空数据库，用于在测试用例中持久化值。

现在我们知道了如何在 Django 中测试模型，在下一节中，我们将学习如何在 Django 中测试视图。

## 测试 Django 视图

Django 中的视图根据用户在 Web 应用程序中访问的 URL 控制 HTTP 响应的渲染。在本节中，我们将学习如何在 Django 中测试视图。假设你正在开发一个网站，其中需要大量应用程序编程接口 API 端点。一个有趣的问题是，你将如何验证每个新端点？如果手动操作，每次添加新端点时你都必须先部署应用程序，然后手动在浏览器中访问该端点以验证其是否正常工作。这种方法在端点数量较少时可能可行，但在端点数以百计时可能会变得极其繁琐。

Django 提供了非常全面的应用程序视图测试方式。这是通过使用 Django 测试模块提供的测试客户端类来实现的。此类可用于访问映射到视图的 URL 并捕获访问 URL 端点时生成的输出。然后，我们可以使用捕获的输出来测试 URL 是否生成正确的响应。此客户端可通过从 Django 测试模块导入 Client 类然后对其进行初始化来使用，如以下代码段所示：

```python
from django.test import Client

c = Client()
```

Client 对象支持多种方法，可用于模拟用户可以发出的不同 HTTP 调用——即 GET、POST、PUT、DELETE 等。进行此类请求的一个示例如下所示：

```python
response = c.get('/welcome')
```

视图生成的响应随后会被客户端捕获并作为响应对象暴露出来，然后可以查询该对象以验证视图的输出。

有了这些知识，让我们来看看如何为 Django 视图编写测试用例。

### 练习 14.03 – 为 Django 视图编写单元测试

在本练习中，我们将使用 Django 测试客户端为映射到特定 URL 的 Django 视图编写测试用例。这些测试用例将帮助你验证在通过其映射的 URL 访问时，View 函数是否生成正确的响应。让我们开始逐步操作：

1. 对于本练习，你将使用在 *练习 14.02 – 测试 Django 模型* 的 *步骤 1* 中创建的 `bookr_test` 应用程序。开始时，打开 `bookr_test` 目录下的 `views.py` 文件并向其中添加以下代码：

```python
from django.http import HttpResponse

def greeting_view(request):
    """Greet the user."""
    return HttpResponse("Hey there, welcome to Bookr!\n    Your one stop place to review books.")
```

在这里，你创建了一个简单的 Django 视图，用于在用户访问映射到所提供视图的端点时向用户发送欢迎问候。

2. 创建视图后，你需要将其映射到可在浏览器或测试客户端中访问的 URL 端点。为此，打开 `bookr_test` 目录下的 urls.py 文件并将高亮代码添加到 urlpatterns 列表中：

```python
from django.urls import path
from . import views

urlpatterns = [
    path('test/greeting', views.greeting_view,
        name='greeting_view')
]
```

在前面的代码片段中，我们通过在 urlpatterns 列表中设置路径，将 greeting_view 映射到应用程序的 `'test/greeting'` 端点。

3. 设置此路径后，我们需要确保它也能被我们的项目识别。为此，我们需要将此条目添加到 Bookr 项目的 URL 映射中。为实现这一点，打开 bookr 目录中的 urls.py 文件并将以下高亮行附加到 urlpatterns 列表的末尾：

```python
urlpatterns = [
    '...',
    '...',
    path('', include('bookr_test.urls'))
]
```

4. 设置视图后，验证其是否正常工作。通过运行以下命令来执行此操作：

```
python manage.py runserver localhost:8080
```

然后，在你的 Web 浏览器中访问 http://localhost:8080/test/greeting。页面打开后，你应该会看到在步骤 1 中添加到 greeting view 的以下文本显示在浏览器中：

```
Hey there, welcome to Bookr! Your one stop place to review books.
```

5. 现在，我们准备为 `greeting_view` 编写测试用例。对于本练习，我们将编写一个测试用例，检查在访问 `/test/greeting` 端点时是否获得成功结果。为实现此测试用例，打开 `bookr_test` 目录下的 `tests.py` 文件并将以下代码添加到文件末尾：

```python
from django.test import TestCase, Client

class TestGreetingView(TestCase):
    """Test the greeting view."""
    def setUp(self):
        self.client = Client()

    def test_greeting_view(self):
        response = self.client.get('/test/greeting')
        self.assertEquals(response.status_code, 200)
```

在前面的代码片段中，我们定义了一个测试用例，帮助验证 greeting view 是否正常工作。

这是通过首先导入 Django 的测试客户端来实现的，该客户端允许你通过对其进行调用并分析生成的响应来测试映射到 URL 的视图：

```python
from django.test import TestCase, Client
```

导入完成后，我们必须创建一个名为 `TestGreetingView` 的新类，用于对在 *步骤 2* 中创建的 greeting view 相关的测试用例进行分组：

```python
class TestGreetingView(TestCase):
```

在此测试用例中，我们定义了两个方法，`setUp()` 和 `test_greeting_view()`。`test_greeting_view()` 方法实现了你的测试用例。在其中，我们首先向映射到 greeting view 的 URL 发出 HTTP GET 调用，并将视图生成的响应存储在创建的 response 对象中：

```python
response = self.client.get('/test/greeting')
```

此调用完成后，其 HTTP 响应代码、内容和标头将可在 response 变量中使用。接下来，通过以下代码，我们进行断言，验证调用生成的状态码是否与成功 HTTP 调用的状态码 HTTP 200 匹配：

```python
self.assertEquals(response.status_code, 200)
```

至此，我们已准备好运行测试。

幸运的是，Django 的测试客户端提供了此功能，通过它我们可以登录到我们的视图然后对其运行测试。可以使用 Django 的测试客户端 `login()` 方法实现此结果。当此方法被调用时，Django 的测试客户端会对服务执行身份验证操作。如果身份验证成功，它会将登录 cookie 内部存储起来，以便在后续测试中使用。以下代码片段展示了如何设置 Django 的测试客户端以模拟已登录用户：

```python
login = self.client.login(username='testuser',
                          password='testpassword')
```

login 方法需要用于测试的测试用户的用户名和密码，如下一练习所示。因此，让我们看看如何测试需要用户身份验证的流程。

### 练习 14.04 – 编写测试用例以验证已认证用户

在本练习中，我们将为需要用户身份验证的视图编写测试用例。作为其一部分，我们将验证当未登录用户尝试访问页面时以及当已登录用户尝试访问映射到视图函数的页面时，视图方法生成的输出。让我们开始逐步操作：

1. 对于本练习，我们将使用你在练习 14.02 – 测试 Django 模型 的步骤 1 中创建的 `bookr_test` 应用程序。开始时，打开 `bookr_test` 应用程序下的 views.py 文件并向其中添加以下代码：

```python
from django.http import HttpResponse
from django.contrib.auth.decorators import login_required
```

2. 添加前面的代码片段后，在文件末尾创建一个名为 `greeting_view_user()` 的新函数，如以下代码片段所示：

```python
@login_required
def greeting_view_user(request):
    """Greeting view for the user."""
    user = request.user
    return HttpResponse("Welcome to Bookr!\n{username}".format(username=user))
```

在这里，我们创建了一个简单的 Django 视图，用于在已登录用户访问映射到所提供视图的端点时向其发送欢迎问候。

