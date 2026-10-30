

在编写测试用例时，你不可避免地会遇到模拟对象。`Mocking` 是测试领域最受欢迎的概念之一，开发者用它来编写单元/集成测试，随着应用程序逻辑/代码变得复杂，开发者倾向于在复杂的代码片段中使用模拟。

`Mock` 是 Python `unittest` 框架中的内置包。在构建 Web 应用程序时，我们会有复杂的逻辑或外部系统集成，或与应用程序不同部分交互的代码块。为此类代码块编写测试用例很困难，可能需要大量精力。使用 `mock`，我们可以将想要测试的系统的某些部分替换为模拟对象。当我们的代码与外部系统交互时，或我们的应用程序代码需要大量精力进行初始设置时，模拟是有益的。我们不会重点解释如何使用模拟，因为这是一个非常广泛的话题，我们需要单独一章来解释模拟的概念和用例。

阅读 Python `unittest` 官方文档中关于 Python 测试库中模拟概念的更多内容：[https://docs.python.org/3/library/unittest.mock.html](https://docs.python.org/3/library/unittest.mock.html)

### 主观想法

经常，开发者倾向于追逐代码测试覆盖率指标，只是为了满足这些指标而编写测试。增加任何复杂代码覆盖率的最简单方法是通过编写会模拟复杂逻辑的单元测试；这会导致大量脆弱的测试或伪测试，从而放弃编写系统测试的原始用例。建议在编写测试用例时尽可能避免使用 `mock`，编写能够体现实际生产系统的实际测试。这样，你就不会仅仅为了数字而追逐覆盖率指标，测试代码覆盖率百分比代表了代码的真实状态。

在现实生活中，在生产环境中，我们会看到 `mock` 的广泛使用。因此，建议开发者做出判断何时使用它，并深入理解模拟的根深蒂固的问题和概念，以便编写更好的代码。以下是一些我推荐给任何想要了解如何使用 mock 编写测试用例的人的资源：

*   [https://www.youtube.com/watch?v=ww1UsGZV8fQ](https://www.youtube.com/watch?v=ww1UsGZV8fQ)：解释在编写测试时修补函数时幕后发生了什么
*   [https://www.youtube.com/watch?v=rk-f3B-eMkI](https://www.youtube.com/watch?v=rk-f3B-eMkI)：解释我们如何停止使用模拟，这可以帮助我们重新思考编写良好设计的应用程序代码的方式
*   [https://www.youtube.com/watch?v=Ldlz4V-UCFw](https://www.youtube.com/watch?v=Ldlz4V-UCFw)：解释在不过度使用模拟的情况下编写测试的不同方法

在下一节，让我们学习如何为我们的 Celery 应用程序编写测试用例。Celery 是我们 Django 应用程序不可或缺的一部分，因此始终建议为我们的 celery 任务的业务逻辑编写测试用例。

### 为 Celery 编写测试

在 [*第 8 章*](#B18867_08.xhtml#_idTextAnchor169)，我们学习了如何使用 Celery 在 Django 中为长时间运行的代码块编写代码。Celery 是业务用例不可或缺的一部分，因此我们应该有覆盖 Celery 任务的测试用例。Celery 任务只不过是一个普通的函数，具有额外的装饰器，将函数转换为 Celery 任务。

Celery 官方文档中有一个部分提到我们如何在 Celery 中编写任务：[https://docs.celeryq.dev/en/stable/userguide/testing.html](https://docs.celeryq.dev/en/stable/userguide/testing.html)。官方文档也建议不要对任何单元测试使用 `Eager mode`。然而，有时，使用 Eager 比官方文档建议的模拟 Celery 任务更好。因此，为 Celery 任务编写测试在很大程度上是一个选择，开发者需要在仔细考虑项目和专业知识后决定是否使用模拟或 Eager 模式。

#### 主观想法

由于 Celery 任务是常规任务，大多数时候我建议编写更多断言 Celery 任务业务逻辑的单元测试。为了测试任何任务的启动，我建议简单地模拟任务调用并验证模拟任务是否按预期行为被调用。

除了 Celery 任务之外，我们还学习了如何在 [*第 7 章*](#B18867_07.xhtml#_idTextAnchor157) 中使用 Django Signals 框架解耦我们的代码。在下一节中，我们将学习如何为 Django 信号和接收器编写测试用例。

### 为信号和接收器编写测试

`Signals` 是 Django 提供的一个重要功能。Django 信号为我们提供了一个抽象层，这也是编写信号和接收器测试用例很棘手的主要原因。由于接收器是独立的函数，在任何信号上触发，因此很容易通过为 `receiver` 函数编写单元测试来为它们编写单元测试。

Django 文档中没有推荐的方法说明你应该如何测试信号。因此，不同的开发者找到了多种编写测试的方法。我通常用来测试接收器连接的一种方法是将我们想要测试的接收器函数断开连接，然后验证接收器是否已断开连接。这是什么意思？让我们举个例子，`my_handler` 接收器连接到 `post_save` Django 模型信号。创建一个新文件 `author/receivers.py`，并添加以下代码：

```
from django.db.models.signals import pre_save
from django.dispatch import receiver
from author.models import Author
@receiver(post_save, sender=MyModel)
def my_handler(sender, **kwargs):
    print('my handle receiver')
```

现在，我们想测试我们的接收器函数是否成功连接到 `Author` 的 `post_save` 信号。我们将使用反转原则来测试此场景；接收器只有在连接到正确的信号时才能断开连接——用一个类比来说，你只能在灯泡打开时关闭灯泡。将以下代码添加到 `author/tests/tests_signals.py` 文件中：

```
class SignalsTest(TestCase):
    def test_connection(self):
        result = signals.post_save.disconnect(
           receiver=my_handler, sender=Author
        )
        self.assertTrue(result)
```

阅读有关 `disconnect` 函数的更多信息，请参阅 Django 官方文档：[https://docs.djangoproject.com/en/stable/topics/signals/#disconnecting-signals](https://docs.djangoproject.com/en/stable/topics/signals/#disconnecting-signals)。

使用测试和信号的另一个重要概念是为测试用例停止 Django 信号。通过在测试用例期间在不需要时禁用信号，我们可以更快地运行测试用例。例如，如果我们编写了应用程序逻辑，其中创建 `User` 对象会自动触发模型信号，从而创建更多数据库条目并执行数据库操作。如果我们编写的单元测试不会测试或影响信号引起的逻辑更改，那么我们的测试将自动运行缓慢。

为了解决这个问题，我们可以使用 `factory_boy` 来静音与 Django 模型或任何额外信号相关的所有信号。我们可以使用上下文管理器与 `factory_boy` 提供的 `mute_signals` 接口，以确保在我们创建新模型对象时不会触发任何信号：

```
from factory.django import mute_signals
class SignalsTest(TestCase):
    def test_demo(self):
        with mute_signals(signals.pre_save, signals.post_save):
            # pre_save/post_save won't be called here.
            return SomeFactory(), SomeOtherFactory()
```
