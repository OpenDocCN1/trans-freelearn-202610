

默认情况下，Django 提供了大量中间件，我们也在 [*第三章*](#B18867_03.xhtml#_idTextAnchor067) 中学习了 DRF 如何利用 Django 中间件框架来集成 REST API。默认的 Django 中间件主要用于身份验证，并且与安全相关。当我们创建一个新项目时，Django 会在 `settings.py` 文件中自动插入此中间件：

```
MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]
```

需要注意的是，这些中间件项在 `settings.py` 文件中定义的顺序，就是请求到来时被评估的顺序，而响应发送回客户端时则是相反的顺序。

Django 中间件利用了简单的 Python 类概念。`middleware` 类在 Django 服务器启动时实例化，然后对于每个请求，它都会执行 `__call__` 方法内部编写的所有代码。Django 还提供了一种创建我们自己的自定义中间件的方法，可以轻松地将其插入到系统中。让我们学习如何在 Django 中创建自定义中间件。

# 创建自定义中间件

自定义 Django 中间件应用于执行我们希望对所有请求/响应运行的代码。例如，我们希望为所有请求/响应添加自定义头，或者我们希望为请求添加日志记录或监控。在 [*第六章*](#B18867_06.xhtml#_idTextAnchor145) 中，我们学习了如何使用自定义中间件为每个请求添加 `get user_id` 用于日志记录。在本节中，我们将学习 Django 自定义中间件的核心概念。

让我们现在检查如何在 Django 中创建自定义中间件：

1. 创建一个名为 `custom_middleware.py` 的文件（也可以使用其他名称）。将以下代码添加到 `common/custom_middleware.py` 文件中：

```
class CustomMiddleware:
    def __init__(self, get_response):
      self.get_response = get_response
    def __call__(self, request):
      print("custom middleware before request view")
      # Code to be executed for each request
      response = self.get_response(request)
      # Code to be executed for each response
      print("custom middleware after response view")
      return response
```

在前面的代码片段中，我们有两个 `print` 语句来描述自定义中间件。在我们的示例中，我们添加了一个 `print` 语句，但我们可以扩展此概念来实现任何逻辑。我们也可以使用中间件修改 `request` 或 `response` 对象。例如，Django 使用默认的身份验证中间件将 `user` 属性添加到 `request` 对象。

需要注意 `response = self.get_response(request)` 代码，它负责确保我们将代码执行发送到下一个中间件或请求视图逻辑以供执行。`self.get_response` 给出的结果是视图或前一个中间件返回的响应。

2. 将自定义中间件添加到 `settings.py` 文件中的中间件配置中。我们添加中间件的顺序非常重要：

```
MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",
    "django.contrib.sessions.middleware.SessionMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.csrf.CsrfViewMiddleware",
    "django.contrib.auth.middleware.AuthenticationMiddleware",
    "django.contrib.messages.middleware.MessageMiddleware",
    "django.middleware.clickjacking.XFrameOptionsMiddleware",
    # Custom middleware
    "common.custom_middleware.CustomMiddleware",
]
```

对于每个请求，我们的 `CustomMiddleware` 类现在将被执行。为了验证 `CustomMiddleware` 集成，运行 Django 服务器，然后打开浏览器上的任何 [URL（例如，`http`](http://127.0.0.1:8000/blog/paginated/)**://127.0.0.1:8000/blog/paginated/**)。我们将在终端中看到以下输出：

```
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
custom middleware before request view
custom middleware after response view
[03/Dec/2023 12:27:31] "GET /blog/paginated/?page=2&page_size=2 HTTP/1.1" 200 10734
```

自定义中间件会在每次向我们的 Django 服务器发出请求时打印出来。

我们的自定义中间件可用于实现除我们已实现的打印之外的不同逻辑。多个第三方包使用中间件概念来修改任何 `request`/`response` 对象。虽然中间件是 Django 中非常重要的概念，但你也应该记住中间件带来的性能开销。中间件会在每个请求上执行，因此我们在编写/使用中间件之前应该了解我们的用例，否则它可能会降低我们的性能。

# 总结

在本章中，我们学习了 Django 中的三个高级概念。我们学习了分页的不同概念，以及 Django 和 DRF 如何利用分页来改进 RESTful API。接下来，我们学习了 Django 信号的概念，以及 Django 如何提供开箱即用的信号来解耦不同的应用程序逻辑。Django 还提供了扩展自定义信号的接口。最后，我们学习了 Django 中间件概念。Django 中间件用于修改 `request`/`response` 对象。我们还学习了如何创建自定义中间件并在我们的 Django 项目中使用它。

在本章中，我们学习了 Django 的所有高级概念，任何开发人员都应该能够舒适地实现这些概念，以便编写可扩展的 Django 应用程序。

在下一章中，我们将重点学习使用 Django 进行分布式任务队列编程。Celery 是最流行的 Python 框架之一，与 Django 一起使用以实现长时间运行的逻辑。随着我们构建应用程序，我们会遇到长时间运行的用例或需要调度的任务。Celery 有助于实现这些长时间运行/周期性任务。

# 8

# 使用 Django 的 Celery

我们在前几章中学习了 Django 和 DRF 提供的开箱即用功能。Django 和 DRF 都为框架提供了定制和可扩展性的范围，但异步编程在 Django 中缺失了相当长一段时间。然而，随着 Django 4 的引入，Django 开始使用不同的异步接口原生支持异步编程。这仍然无法帮助执行长时间运行的作业或按周期间隔执行的作业的需求。为了解决这一需求，我们使用 Python 中一个众所周知的第三方包 `Celery`。

Celery 是一个简单、灵活且可靠的分布式系统，用于处理大量消息。它是一个任务队列，专注于实时处理，同时也支持任务调度。在本章中，我们将学习如何将 Celery 与 Django 结合使用，并使用 `celery beat` 在 Django 中执行周期性任务。由于 Celery 本身就是一个完全不同的框架，我们不会深入研究该框架，而是专注于实现细节，并提供高层次的概述来帮助你入门。

在本章中，我们将学习以下主题：

*   使用 Django 进行异步编程
*   使用 Django 的 Celery
*   使用 Django 的 `celery beat`

# 技术要求

在本章中，我们将介绍异步编程的一些高级概念，因此读者必须精通以下内容：

