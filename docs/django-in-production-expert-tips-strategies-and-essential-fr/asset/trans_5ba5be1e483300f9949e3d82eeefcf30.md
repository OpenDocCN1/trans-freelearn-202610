

*   同步编程与异步编程的区别，以及后者相对于前者的优势。
*   消息代理和任务队列的基本概念。`Celery` 是一个用于处理任务队列和任务调度的框架。
*   如何设置 Redis 并在本地系统中使用远程 Redis 服务器（我们在 [*第 6 章*](#B18867_06.xhtml#_idTextAnchor145) 中讨论过此内容）。

# Django 中的异步编程

Django 4 在其核心框架中引入了异步编程的概念。虽然 Django 引入了对异步视图的支持，但官方文档（[https://docs.djangoproject.com/en/stable/topics/async/#performance](https://docs.djangoproject.com/en/stable/topics/async/#performance)）指出，异步视图会导致性能下降。

因此，截至 Django 4.x，原生的异步编程适配尚未成熟。虽然大多数常见和基本功能不需要任何异步支持，但作为框架，Django 因此落后于 Express 和 Spring 等其他框架。为了解决这个问题，Django 开发者使用第三方框架来实现异步支持。一般来说，当有一个可以在后台执行的长时间运行任务时，异步编程会有所帮助。例如，当我们需要发送一封电子邮件时，我们的 Django 视图会向电子邮件服务发出外部 API 调用并等待响应。整个过程可能需要一段时间，让客户端请求等待电子邮件服务是没有额外价值的；相反，更倾向于在后台执行此任务。对于此类异步的长时间运行/后台任务，可以使用 `Celery` 等框架。`Celery` 是一个分布式任务队列，用于将任务从主要请求-响应工作流中卸载。

注意

应该记住，`Celery` 本身是一个框架，具有自己的功能和复杂的架构。`Celery` 的架构超出了本书的范围。我们将在本章中仅关注 `Celery` 与 Django 的集成。要了解 `Celery` 的架构和高级概念，建议您查阅 `Celery` 官方文档 [https://docs.celeryq.dev/en/stable/index.html](https://docs.celeryq.dev/en/stable/index.html)。

在下一节中，我们将学习如何将 `Celery` 与 Django 一起使用，并利用 `Celery` 的功能在后台执行长时间运行的任务。

# 使用 Celery 与 Django

让我们通过一个小的实际例子来了解 `Celery` 在内部是如何工作的。想象一下，你想执行 10 个不同的任务，而你只有 2 个人来完成它们。每个人一次只能处理一个特定的任务，并且新任务可能在任何时候到来。为了解决这个问题，工作人员会创建一个列表，每当有新任务进来时，他们就会将任务添加到该列表中。每当一个人完成任务时，你就会从列表中划掉该任务，然后为他们分配下一个任务。如果为你增加更多人，那么你就可以更快地完成任务。这就是 `Celery` 所做的。

在 `Celery` 中，有一个任务队列跟踪所有传入的任务 - 任务队列使用 RabbitMQ 或 Redis 保存。有 `Celery` 工作进程从队列中获取任务并处理它们。Django 生成这些任务并将它们推送到任务队列，`Celery` 从中获取任务并使用 `Celery` 工作进程执行它们。

现在让我们看看如何将 `Celery` 集成到 Django 项目中。我们将使用 Redis 来创建和维护项目中的 `Celery` 任务队列。在 *图 8**.1* 中，我们有一个简约的 `Celery`-Django 架构：

![Figure 8.1: Celery architecture with Django](image/Figure_8.1_B18867.jpg) 

图 8.1：Celery 与 Django 的架构

在 Django 中运行的应用服务器创建任务并将这些任务发送到 Redis 任务队列。然后 `Celery` 工作进程一个接一个地从队列中获取这些任务并运行它们。这是一个过度简化的例子，说明 `Celery` 和 Django 在内部是如何工作的，但它应该能让你对架构有一个很好的了解。

现在让我们看看如何安装 `Celery` 并将其集成到我们的系统中。

## 集成 Celery 和 Django

`Celery` 提供了多种代理集成，如 RabbitMQ、Redis 和 Amazon SQS。我们将使用 Redis 作为 `Celery` 服务的代理。在 [*第 6 章*](#B18867_06.xhtml#_idTextAnchor145) 中，我们学习了如何在本地使用远程 Redis 服务器，并且我们将在此处为我们的 `Celery` 代理使用相同的 Redis 服务器。让我们看看将 `Celery` 集成到 Django 的步骤：

1. 作为第一步，我们需要使用 `pip install celery` 命令在项目中安装 `Celery`。

2. 在与 `settings.py` 文件相同的文件夹中创建一个新的 `celery.py` 文件，其中包含以下代码（对于我们来说，位置是 `backend/config/celery.py`）：

```
import os
from celery import Celery
celery_settings_value = "<project name>.settings"
# change <project name> with folder name where your settings.py file is present.
os.environ.setdefault("DJANGO_SETTINGS_MODULE", celery_settings_value)
app = Celery("<project name>")
# change <project name> with folder name where your settings.py file is present.
app.config_from_object("django.conf:settings", namespace="CELERY")
app.autodiscover_tasks()
task = app.task
@app.task(bind=True)
def debug_task(self, data):
    print(data)
```

3. 现在我们必须将 Redis 代理连接详细信息添加到 `settings.py` 文件中。你需要在 RedisLabs 中更新 Redis 连接详细信息。在此步骤中，我们告诉 `Celery` 使用 Redis 作为代理和结果后端：

```
REDIS_CONNECTION_STRING = '<user>:<pass>@<endpoint>'
# Use redis lab credentials
CELERY_BROKER_URL = f"redis://{REDIS_CONNECTION_STRING}"
CELERY_RESULT_BACKEND = f"redis://{REDIS_CONNECTION_STRING}"
```

我们已经集成了 Django、`Celery` 和 Redis。`Celery` 使用工作进程来执行任务。现在我们需要定义工作进程。就像 Django 管理命令一样，`Celery` 也有一个管理命令，可用于定义工作进程。

4. 打开一个新终端并运行以下命令（请注意 - `<project>` 关键字需要替换为 `celery.py` 文件所在的文件夹名称）：

```
celery --app=<project> worker --loglevel=INFO
```

此命令创建一个可用于在本地开发环境中测试和工作的 `Celery` 工作进程。如果我们的集成成功，我们应该能够看到 *图 8**.2* 中显示的输出：

![Figure 8.2: Terminal running Celery workers](image/Figure_8.2_B18867.jpg) 

图 8.2：运行 Celery 工作进程的终端

一旦我们确认 `Celery` 工作进程正在运行，我们就可以停止工作进程并进入 Django 集成步骤。

1. 为了测试我们的集成，让我们从 Django 视图调用我们的 `Celery` 任务。将以下代码添加到 `blog/views.py` 文件中：

```
from config.celery import debug_task
@api_view(['GET'])
def verify_blog(request):
    verify_word = request.GET.get('verify_word')
    debug_task.delay(f"Celery Task verification: {verify_word}")
    return Response({'status': 'success'})
```

*请注意，我们从 `config.celery` 导入 `debug_task`，因为我们已在 `config` 文件夹/应用中创建了 `celery.py` 文件。你的版本可能有其他名称，因此请确保检查。*

现在我们需要将视图链接到 URL。将以下代码添加到 `blog/urls.py` 文件中：

```
urlpatterns = [
    path('verify/', views.verify_blog)
]
```

我们已成功将 `Celery` 集成到我们的 Django 项目中。

现在为了验证我们的 `Celery` 集成是否按预期工作，让我们在终端上运行 `Celery` 工作进程，运行以下命令：

```
celery --app=config worker --loglevel=INFO
```

