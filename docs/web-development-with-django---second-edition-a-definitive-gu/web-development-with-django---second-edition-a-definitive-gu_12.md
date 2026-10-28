## 测试你的 Django 应用

在前几章中，我们专注于通过编写数据库模型、视图和模板等不同组件，在 Django 中构建 Web 应用。我们这样做是为了给用户提供一个交互式应用，让他们能够创建个人资料并为他们读过的书撰写书评。

除了构建和运行应用之外，还有一个重要方面：确保应用代码按我们期望的方式工作。这可以通过一种称为**测试**的技术来保证。在测试中，我们运行 Web 应用的不同部分，并检查所执行组件的输出是否与预期输出匹配。如果输出匹配，我们就说该组件测试成功；如果输出不匹配，我们就说该组件未能按预期工作。

在本章中，随着我们浏览不同的小节，我们将学习测试为何重要、测试 Web 应用的不同方法，以及如何构建一个强大的测试策略，以帮助我们确保所构建的 Web 应用是健壮的。

在本章中，我们将介绍以下主题：

- 测试的重要性
- 自动化测试
- Django 中的测试
- 测试 Django 模型
- 测试 Django 视图
- Django 请求工厂
- Django 中的测试用例类

让我们先从了解测试的重要性开始我们的旅程。

### 技术要求

本章的完整代码文件可以在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter14 找到。

### 测试的重要性

确保应用按设计的方式工作是开发工作中的一个重要方面，否则我们的用户可能会不断遇到奇怪的行为，这通常会降低他们对应用的参与度。

我们在测试上付出的努力有助于确保我们打算解决的各种问题确实得到正确解决。想象一下，一个开发人员正在构建一个在线事件调度平台。在这个平台上，用户可以根据当地时区在日历上安排事件。现在，如果在这个平台上，用户能够按预期安排事件，但由于一个 bug，事件被安排到了错误的时区，会怎么样？正是这类问题往往会赶走许多用户。

这就是为什么许多企业公司花费大量资金来确保他们正在构建的应用经过了全面测试。这样，他们就能确保不会发布有缺陷的产品，或者远远不能满足用户需求的产品。

简而言之，测试帮助我们实现以下目标：

- 确保应用组件按照规范工作
- 确保在不同基础设施平台上的互操作性，例如应用可以部署在 Linux 或 Windows 等不同操作系统上
- 有助于降低在重构应用代码时引入 bug 的概率

现在，许多人对测试的一个常见设想是，他们必须在组件开发时手动测试所有组件，以确保每个组件符合其规范，并在每次更改或向应用中添加新组件时重复这项工作。虽然这没错，但这并不能提供测试的完整图景。随着时间推移，测试作为一项技术已经变得非常强大，作为开发人员，你可以通过实现自动化测试用例来减少大量测试工作。那么，这些自动化测试用例是什么呢？或者换句话说，什么是自动化测试？让我们来了解一下。

### 自动化测试

当单个组件被修改时，反复测试整个应用可能会变成一项具有挑战性的任务，如果该应用包含大型代码库，则更是如此。代码库的规模可能归因于功能的绝对数量或它解决问题的复杂性。

### 实现测试用例

在着手实现用于测试代码的机制时，你需要理解的第一件事是如何对这种实现进行逻辑分组，以便彼此密切相关的模块在一个逻辑单元中进行测试。

实现**测试用例**可以简化这一过程。测试用例不过是一个逻辑单元，将相关的测试分组在一起，以便所有为测试用例初始化环境的通用逻辑可以合并到同一处，从而避免在实现应用测试代码时重复工作。

### Django 中的单元测试

既然我们已经理解了测试，让我们来看看如何在 Django 中进行单元测试。在 Django 的上下文中，单元测试由两个主要部分组成：

- 一个 `TestCase` 类，它封装了为给定模块分组的不同测试用例
- 一个实际需要执行的测试用例，用于测试特定组件的流程

实现单元测试的类应继承自 Django 的 `test` 模块提供的 `TestCase` 类。默认情况下，Django 在每个应用目录中都提供了一个 `tests.py` 文件，可用于存储应用模块的测试用例。

一旦编写好这些单元测试，就可以直接使用 `manage.py` 中提供的 `test` 命令轻松运行它们，如下所示：

```
python manage.py test
```

现在，让我们看看断言如何帮助我们构建单元测试并检查预期行为。

### 利用断言

编写测试的一个重要部分是验证测试是通过还是失败。通常，为了在测试环境中做出此类判断，我们会使用一种称为**断言**的东西。

断言是软件测试中的一个常见概念。它们接收两个操作数，并验证**左侧（LHS）**操作数的值是否与**右侧（RHS）**操作数的值匹配。如果 LHS 的值与 RHS 的值匹配，则断言被视为成功；如果值不同，则断言被视为失败。

评估结果为 `False` 的断言本质上会使测试用例被评估为失败，然后向用户报告。

Python 中的断言实现起来相当容易，它们使用一个名为 assert 的简单关键字。例如，以下代码片段展示了一个非常简单的断言：

```
assert 1 == 1
```

上述断言接收一个表达式，该表达式求值为 True。如果这个断言是测试用例的一部分，那么测试就会成功。

现在，让我们看看如何使用 Python unittest 库实现测试用例。这样做非常简单，可以通过一些易于遵循的步骤来完成：

1. 导入 unittest 模块，它允许我们构建测试用例：

```
import unittest
```

2. 导入模块后，我们可以创建一个类，其名称以 Test 开头，并继承自 unittest 模块提供的 TestCase 类：

```
class TestMyModule(unittest.TestCase):
    def test_method_a(self):
        assert <expression>
```

只有当 TestMyModule 类继承 TestCase 类时，Django 才能自动运行它，并与框架完全集成。定义类之后，我们可以在类中实现一个名为 test_method_a() 的新方法，用于验证断言。就是这样。

这里需要注意的一个重要部分是测试用例和测试函数的命名规范。实现的测试用例应以 Test 名称为前缀，以便测试执行模块能够将它们识别为有效测试用例并执行它们。同样的规则也适用于测试方法的命名。

3. 编写完测试用例后，只需运行以下命令即可执行：

```
python manage.py test
```

在明确了实现测试用例的基本思路之后，让我们编写一个非常简单的单元测试，来感受一下 Django 中单元测试框架的行为。




# 练习 14.01 – 编写一个简单的单元测试

在本练习中，我们将编写一个简单的单元测试来了解 Django 单元测试框架的工作原理，并利用这一知识来实现我们的第一个测试用例，该用例验证一个简单的表达式：

+   1. 首先，打开 Bookr 项目中 reviews 应用下的 tests.py 文件。默认情况下，该文件将只包含一行代码，即从 test 模块导入 Django 的 TestCase 类：

```
from django.test import TestCase
```

+   2. 在刚刚打开的 tests.py 文件中添加以下代码行：

```
class TestSimpleComponent(TestCase):
    def test_basic_sum(self):
        assert 1+1 == 2
```

这里，我们创建了一个名为 TestSimpleComponent 的新类，它继承自 Django 测试模块提供的 TestCase 类。assert 语句将比较 LHS 上的表达式 (1+1) 与 RHS 上的表达式 (2)。

+   3. 编写完测试用例后，返回到项目文件夹并运行以下命令：

```
python manage.py test
```

应生成以下输出：

```
% ./manage.py test
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.
----------------------------------------------------
-----
Ran 1 test in 0.001s

OK
Destroying test database for alias 'default'...
```

前面的输出表明 Django 的测试运行器执行了 1 个测试用例，该用例成功通过了评估。

+   4. 在确认测试用例正常工作并通过后，我们现在将在 test_basic_sum() 方法的末尾添加另一个断言，如以下代码片段所示：

```
assert 1+1 == 3
```

第二个是 tearDown() 方法。该方法在测试函数执行完毕后被调用，并在测试用例执行结束后清理变量及其值。无论测试用例评估为 True 还是 False，该方法都会被执行。使用 tearDown() 方法的示例如下所示：

```
class MyTestCase(unittest.TestCase):
    def setUp(self):
        # Do some initialization work
    def test_method_a(self):
        # code for testing method A
    def test_method_b(self):
        # code for testing method B
    def tearDown(self):
        # perform cleanup
```

在前面的示例中，tearDown() 方法将在每次测试方法执行完毕后被调用——即在 test_method_a() 执行完毕后调用一次，并在 test_method_b() 执行完毕后再次调用。

现在我们了解了编写测试用例的不同组件。让我们看看如何使用提供的测试框架来测试 Django 应用的不同方面。

## 测试 Django 模型

Django 中的模型是数据将在应用数据库中存储方式的基于对象的表示。它们提供可帮助我们验证给定记录的数据输入的方法，以及在数据插入数据库之前对数据进行任何处理。

在 Django 中测试模型与创建它们一样简单。现在，让我们看看如何使用 Django 测试框架测试 Django 模型。

## 练习 14.02 – 测试 Django 模型

在本练习中，我们将创建一个新的 Django 模型并为其编写测试用例。测试用例将验证你的模型是否能正确地将数据插入数据库并从数据库中检索数据。当一支开发团队在大型项目上协作时，同一个数据库模型可能会随着时间的推移被多名开发人员修改，这类在数据库模型上工作的测试用例会很有用。实现数据库模型的测试用例可以让开发人员预先识别他们可能在工作中无意中引入的潜在破坏性更改。让我们开始步骤：

+   1. 创建一个新的应用，用于本章的练习。为此，运行以下命令，它将为你的用例设置一个新的应用：

```
python manage.py startapp bookr_test
```

2. 为了确保 bookr_test 应用的行为与 Django 项目中的任何其他应用相同，将此应用添加到 Bookr 项目的 INSTALLED_APPS 部分。为此，打开 Bookr 项目中的 settings.py 文件并将以下代码附加到 INSTALLED_APPS 列表中：

```
INSTALLED_APPS = [
    '...',
    '...',
    'bookr_test'
]
```

3. 现在应用设置完成，创建一个新的数据库模型用于测试目的。对于本练习，我们将创建一个名为 Publisher 的新模型，它将存储我们数据库中图书出版商的详细信息。要创建模型，请打开 bookr_test 目录下的 models.py 文件并向其中添加以下代码：

```
from django.db import models

class Publisher(models.Model):
    """A company that publishes books."""
    name = models.CharField(
        max_length=50, help_text="The name of the Publisher.")
    website = models.URLField(
        help_text="The Publisher's website.")
    email = models.EmailField(
        help_text="The Publisher's email address.")

    def __str__(self):
        return self.name
```

在前面的代码片段中，你创建了一个名为 Publisher 的新类，它继承自 Django models 模块的 Model 类，将该类定义为将用于存储出版商数据的 Django 模型：

```
class Publisher(models.Model)
```

在此模型内部，我们添加了三个字段，它们将作为模型的属性：

+   - `name`: 出版商的名称
+   - `website`: 出版商的网站
+   - `email`: 出版商的电子邮件地址

完成此操作后，我们创建了一个名为 `__str__()` 的类方法，它定义了 Model 类的字符串表示形式将是什么样的。

+   4. 现在模型已创建，在对它运行测试之前需要先迁移此模型。为此，运行以下命令：

```
```bash
python manage.py makemigrations
python manage.py migrate
```
```

+   5. 模型现在已设置好，编写用于测试你在 *步骤 3* 中创建的模型的测试用例。为此，打开 `bookr_test` 目录下的 `tests.py` 文件并向其中添加以下代码：

```
```python
from django.test import TestCase
from .models import Publisher


class TestPublisherModel(TestCase):
    """Test the publisher model."""
    def setUp(self):
        self.p = Publisher(name='Packt',
                           website='www.packt.com',
                           email='contact@packt.com')

    def test_create_publisher(self):
        self.assertIsNotNone(self.p, Publisher)

    def test_str_representation(self):
        self.assertEquals(str(self.p), "Packt")
```
```

在前面的代码片段中，有几件事值得探讨。

开始时，在从 Django 测试模块导入 TestCase 类后，你从 bookr_test 目录导入了 Publisher 模型，它将用于测试。

导入所需库后，我们必须创建一个名为 TestPublisherModel 的新类，它继承 TestCase 类并用于对与 Publisher 模型相关的单元测试进行分组：

```
class TestPublisherModel(TestCase):
```

在此类内部，我们定义了几个方法。首先，我们定义了一个名为 setUp() 的新方法，并向其中添加了 Model 对象创建代码，以便在该测试用例内部每次执行新的测试方法时创建 Model 对象。此 Model 对象存储为类成员，以便可以在其他方法中无问题地访问它：

```
def setUp(self):
    self.p = Publisher(name='Packt',
                    website='www.packt.com',
                    email='contact@packt.com')
```

第一个测试用例验证 Publisher 模型的 Model 对象是否成功创建。为此，我们创建了一个名为 test_create_publisher() 的新方法，在其中检查创建的 Model 对象是否指向 Publisher 类型的对象。如果此 Model 对象未成功创建，你的测试将失败：

```
def test_create_publisher(self):
    self.assertIsInstance(self.p, Publisher)
```

如果你仔细检查，你会看到我们在这里使用 unittest 库的 assertIsInstance() 方法来断言 Model 对象是否属于 Publisher 类型。

下一个测试验证模型的字符串表示是否与我们期望的一致。从代码定义来看，Publisher 模型的字符串表示应输出出版商的名称。为了测试这一点，我们创建了一个名为 test_str_representation() 的新方法，并检查生成的模型字符串表示是否与我们期望的一致：

```
def test_str_representation(self):
    self.assertEquals(str(self.p), "Packt")
```

为了执行此验证，我们使用了 unittest 库的 assertEquals 方法，它验证传递给它的两个值是否相等。

6. 测试用例现在已就位，你可以运行它们来查看会发生什么。要运行这些测试用例，运行以下命令：

```
python manage.py test
```



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




## SimpleTestCase 类

此类派生自 Django 测试模块提供的 `TestCase` 类，应在编写用于测试视图函数的简单测试用例时使用。通常，当测试用例涉及数据库查询时，不建议使用此类。该类还提供了许多实用功能，例如：

- 检查视图函数引发的异常
- 测试表单字段
- 内置测试客户端
- 验证视图函数执行的重定向
- 匹配视图函数生成的两个 HTML、JSON 或 XML 输出是否相等

现在我们已经基本了解了 `SimpleTestCase` 类，接下来继续了解另一种测试用例类，它可以帮助我们编写与数据库交互相关的测试用例。

## TransactionTestCase 类

此类派生自 `SimpleTestCase` 类，应在编写涉及数据库交互的测试用例时使用，例如数据库查询、创建模型对象等。

该类提供了以下附加功能：

- 在测试用例运行前将数据库重置为默认状态
- 可以根据数据库功能跳过测试；如果用于测试的数据库不支持生产数据库的全部功能，这一功能会非常方便

## LiveServerTestCase 类

此类与 `TransactionTestCase` 类类似，但有一个小区别：使用此类编写的测试用例不使用默认测试客户端，而是使用由 Django 创建的实时服务器。

在编写用于测试已渲染网页及其交互的测试用例时，运行实时服务器的能力非常有用，因为使用默认测试客户端无法完成这类测试。

此类测试用例可以利用 Selenium 等工具，通过与已渲染页面交互来修改页面状态，从而构建交互式测试用例。

## 模块化测试代码

在前面的练习中，我们了解了如何为项目的不同组件编写测试用例。但需要注意的是，到目前为止，我们一直将所有组件的测试用例都写在同一个文件中。

当应用程序中的视图和模型数量不多时，这种方式没有问题。但随着应用程序不断增长，这种方式可能会带来问题，因为此时单个 `tests.py` 文件将难以维护。

为了避免出现这种情况，我们应尝试将测试用例模块化，将模型测试用例与视图及其他属性相关的测试用例分开保存。要实现模块化，只需执行以下两个简单步骤：

- 1. 在应用程序目录中运行以下命令，创建一个名为 tests 的新目录：

```
mkdir tests
```

- 2. 在 tests 目录中运行以下命令，创建一个名为 __init__.py 的新空文件：

```
touch __init__.py
```

Django 需要这个 __init__.py 文件，才能正确识别我们创建的 tests 目录，将其视为模块而不是普通目录。

完成上述步骤后，就可以为应用程序中的不同组件创建新的测试文件。例如，要为模型编写测试用例，可以在 tests 目录中创建一个名为 test_models.py 的新文件，并将与模型测试相关的代码添加到该文件中。

此外，运行测试时不需要执行其他额外步骤。相同的命令也可以正常运行模块化的测试代码：

```
python manage.py test
```

至此，我们已经了解了如何为项目编写测试用例。那么，不如应用这些知识，为正在开发的 Bookr 项目编写测试用例吧？

## 总结

本章学习了如何使用 Django 为 Web 应用程序项目的不同组件编写测试用例。我们了解了测试为何在任何 Web 应用程序的开发过程中都发挥着关键作用，也了解了业界采用的不同测试技术，以确保发布的应用程序代码稳定且没有错误。

然后，我们了解了如何使用 Django 测试模块提供的 TestCase 类来实现单元测试，该类可用于测试模型和视图。我们还了解了如何使用 Django 测试客户端测试视图函数，其中一些视图函数要求用户经过身份验证。此外，我们还简要介绍了使用 RequestFactory 测试基于方法的视图和基于类的视图的另一种方法。

最后，我们通过了解 Django 提供的预定义类及其适用场景结束了本章，同时还了解了如何将测试代码模块化，使其结构更加整洁。

进入下一章后，我们将了解如何通过将第三方库集成到项目中，使 Django 应用程序更加强大。我们将使用这一功能在 Django 应用程序中实现第三方身份验证，让用户能够通过 Google Sign-In、Facebook Sign-In 等常用服务登录应用程序。
