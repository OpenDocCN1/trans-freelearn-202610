# 探索 Django 中的约定

在之前的章节中，我们学习了 Django 的不同功能，以及如何利用这些功能构建可扩展的 Django 应用程序。Django 被认为是有主见的；它没有给开发者太多灵活性。具有主见性导致了 Django 与开发者社区之间爱恨交织的关系，其中许多开发者喜欢 Django 有特定的实现方式，开发者不必过多思考如何使用 Django 构建 Web 服务。然而，许多开发者不喜欢 Django，因为缺乏灵活性和对代码编写方式的过于严格。

在本章中，我们将学习在与 Django 协作时可以遵循的某些约定。您应该注意，本章包含大量我在从事众多 Django 项目和为不同公司提供咨询时学到的有主见的概念。

让我们看看本章将涵盖的主要主题：

* Django 项目的代码结构
* 处理异常和错误
* 使用功能标志
* 配置 Django 部署到生产环境

# 技术要求

我们将学习迄今为止本书中涵盖的不同 Django 主题的约定。您应熟练掌握前面章节中解释的所有主题。

以下是本章所有代码和说明的 GitHub 仓库：[`github.com/PacktPublishing/Django-in-Production/tree/main/Chapter10`](https://github.com/PacktPublishing/Django-in-Production/tree/main/Chapter10)。

# Django 项目的代码结构





当我们在 Django 中启动一个新项目时，Django 管理命令会创建创建 Django 项目所需的基本文件。在 第 1 章 中，我们学习了如何设置我们的 Django 项目并创建不同的 Django 应用。在本节中，我们将汇总我们在前几章中收集到的关于在 Django 中为不同目的使用不同文件的所有信息。

## 按功能创建文件

作为第一次分叉的第一步，我们已经了解到，我们应该在 Django 项目中创建多个 Django 应用。Django 应用的拆分取决于业务逻辑功能；例如，我们的博客项目可以有 `blog` 和 `author` 作为两个不同的 Django 应用。在每个应用内部，我们会将代码拆分到多个文件中。以下是我们在 Django 工作时应考虑的文件推荐列表：

* `urls.py`: 每个 Django 应用都应有一个 `urls.py` 文件，其中包含该应用的所有路由逻辑。
* `api.py`: 我们应该在 `api.py` 文件中添加所有 API 的定义并将其链接到 `urls.py`。许多开发者也使用 `views.py` 来实现相同目的。
* `constants.py`: 我们应该使用 `constants.py` 文件来添加 Django 应用中使用的所有常量。
* `models.py`: 我们应该将所有数据库表定义放在 `models.py` 文件中。Django 已经强制要求对任何数据库定义相关代码使用 `models.py` 文件。
* `serializers.py`: 将所有 DRF 序列化器逻辑添加到 `serializers.py` 文件中。避免在模型文件中添加序列化器逻辑以保持关注点分离。
* `common.py`: 很多时候，我们可能会在同一个 Django 应用中看到逻辑的重复。最好创建一个 `common.py` 文件，其中包含应用使用的所有通用逻辑。
* `utils.py`: 在构建应用时，我们会编写很多实用函数来执行某些操作。例如，如果我们想从文章中修剪空白，我们就会为此目的创建一个 `util` 函数并将其放在 `utils.py` 文件中以便重用。`common.py` 和 `utils.py` 的主要区别在于 `common.py` 可以依赖于其他文件，例如 `models.py`，但 `utils.py` 不应依赖于其他文件。
* `public.py`: 随着项目的成熟，我们会观察到不同 Django 应用之间的相互依赖。保持文件层次结构对于获得流畅的开发者体验非常重要。我们应该将所有需要被其他 Django 应用访问的函数和类保存在 `public.py` 文件中。
* `_private.py`: 我们应该将只能由内部 Django 应用访问的代码保存在 `_private.py` 文件中。`public.py` 和 `_private.py` 文件的主要区别在于 public 文件的函数可以被任何其他 Django 应用访问，而 private 函数只能被同一个 Django 应用中的文件访问。Public 函数可以在内部调用 private 函数，并且它们可以对 private 函数进行包装以将其暴露给另一个 Django 应用。
* `tasks.py`: 我们在 第 8 章 中学习了如何使用 Celery。正如该章所述，我们应该将所有 Celery 任务代码放在 `tasks.py` 文件中。
* `signals.py`: 将所有自定义 Django 信号定义添加到 `signals.py` 文件中。
* `receivers.py`: 将所有 Django receiver 相关代码添加到 `receivers.py` 文件中。
* `admin.py`: 开箱即用，Django 提供了非常有用的 admin 界面。所有与 Django admin 相关的代码都应放在 `admin.py` 文件中。
* `tests`: 在 第 9 章 中，我们看到了如何利用 `factoryboy` 和 `tests` 文件在 Django 中编写测试。所有测试都应放在 `tests` 文件夹中。

我们讨论了 Django 应用内部使用的不同类型的文件。并非必须从一开始就创建所有这些文件，但将其视为构建大型可维护 Django 应用的参考。

随着项目的成熟，即使有了前面列表中提到的单独文件，你也会发现维护代码变得困难。当这种情况发生时，你应该决定是否将 Django 应用拆分为更小的子应用，或将单个文件拆分为模块。例如，如果你的 `public.py` 文件有 500 多行，那么你应该考虑将文件转换为模块，通过创建 `public` 文件夹然后向其中添加 `__init__.py`。一旦将文件转换为模块，你就可以开始将 `public.py` 文件的逻辑拆分为多个更小的文件。这可以应用于本节中提到的任何文件。

注意

根据我在业界的经验，我学到要遵循 500 行规则：如果任何文件包含超过 500 行代码，建议将文件转换为模块并将逻辑拆分为多个更小的文件。

在下一节中，我们将学习如何避免循环依赖以及不同文件如何帮助我们更好地规划代码结构。

## 避免循环依赖

随着项目的增长，循环依赖不可避免。没有单一的保证方法可以避免循环依赖。Python 会自动提示我们代码的某个部分存在循环导入错误。以下是 Python 抛出的循环导入错误示例：

```
ImportError: cannot import name '<name>' from partially initialized module '<module>' (most likely due to a circular import)
```

但我们可以遵循某些准则来避免这一常见问题：

* 切勿从不同应用导入模型。如果需要，使用 `public.py` 文件接口来导入不同应用的模型。这也有助于抽象出不同应用的实现细节。例如，如果在以后对表进行了任何修改，那么我们就不必更新其他 Django 应用的代码；相反，我们需要保证 `public.py` 接口的契约。
* 始终尝试通过分派信号和使用接收器在两个 Django 应用之间进行通信。这有助于避免直接导入其他应用代码。
* 如果我们仍然遇到循环导入，最简单的解决方案是将罪魁祸首文件拆分为两个不同的部分，并确保它们不会直接相互调用。

循环依赖是一个难以解决的问题，而且大多数时候需要定制的方法来解决它。本节中我们学到的概念只是一个大纲，可以帮助你在规划代码时遵循风格指南并更好地构建它。

## 创建一个“common”应用

我们将创建一个名为 `common` 的 Django 应用。该应用将包含可在整个项目中使用的所有通用代码。例如，如果我们有一个用于保存简单配置的通用模型，那么我们应该将给定的应用添加到 `common` 应用中。`common` 应用可以被任何其他 Django 应用导入，但它永远不应导入任何其他 Django 应用。

例如，如果我们需要创建一个可被多个 Django 应用用于获取配置的简单 *键值存储表*，我们可以将键值存储创建在 `common` 应用中。

以下是我在不同 Django 项目中在 `common` 应用中使用的功能列表：

* 存放所有通用自定义异常类，以便其他 Django 应用可以使用它们。
* 包含项目中通用的自定义分页类。





*   添加通用的日志函数，使所有日志都通过同一个函数传递。这有助于在大规模下维护日志，同时也具有灵活更新日志实现而不破坏现有系统的能力。

*   创建自定义中间件。

*   持有基础模型类。例如，每个模型都应具有 `created_at` 和 `updated_at` 字段。创建一个基础模型并将其放在 `common` 应用中，以便所有 Django 应用模型都继承自该基础模型类。

*   使用基础 admin 和自定义 admin 混入。

尽管 `common` 应用非常有帮助，有助于我们遵循 `Don't Repeat Yourself` (`DRY`) 原则，但我们也应注意将什么代码放入 `Common` 应用模块。随着项目的扩展，重新审视 `common` 应用并决定是否创建单独的应用并将所有代码逻辑移至其中可能是一个好主意。

现在，让我们学习如何使用 Django 设置来设置不同的环境。

## 为生产环境使用设置文件

当我们创建一个新的 Django 项目时，Django 管理命令会创建一个 `settings.py` 文件，其中包含所有 Django 配置。但随着项目的增长，我们将为不同的环境，例如 `local`、`dev`、`stage` 和 `prod`，拥有不同的配置。我们应该开始为不同的环境维护不同的文件。

解决此问题的推荐方法是创建以下文件结构：

*   `settings/base.py`：此文件将包含适用于所有环境的通用 Django 设置，例如 `INSTALLED_APPS`。

*   `settings/local.py`：这将包含运行本地开发的 Django 的所有配置。我们需要将 `base.py` 文件中的所有设置导入到 `local.py` 中。

*   `settings/prod.py`：这将包含在生产环境中运行 Django 的所有配置。同样，我们将从 `base.py` 文件导入所有设置。

我们可以根据需要创建多个设置文件。现在我们已将设置文件拆分为多个文件，我们需要告诉 Django 选择合适的设置。有多种方法可以配置 Django 服务器以选择设置文件：

*   使用 `DJANGO_SETTINGS_MODULE`。我们可以从命令行将设置文件路径设置为环境变量。例如，对于 Unix，我们将使用以下命令：

```
export DJANGO_SETTINGS_MODULE=config.settings.local
```

*   使用 `--settings` 标志传递设置文件，同时使用以下 Django 管理命令启动服务器：

```
python manage.py runserver --settings=config.settings.local
```

我们还应记住，我们不会将任何机密硬编码或提交到代码仓库。相反，我们应始终使用环境变量在运行时导入机密值。这在将代码部署到生产环境时尤其重要。Python 有一个 `os` 库可用于导入环境变量。例如，`os.environ.get("SQL_USER", "user")` 将从 `os` 环境变量加载数据库用户：

```
DATABASES = {
    "default": {
        "ENGINE": os.environ.get("SQL_ENGINE"),
        "NAME": os.environ.get("SQL_DATABASE"),
        "USER": os.environ.get("SQL_USER", "user"),
        "PASSWORD": os.environ.get("SQL_PASSWORD", "password"),
        "HOST": os.environ.get("SQL_HOST", "localhost"),
        "PORT": os.environ.get("SQL_PORT", "5432"),
    }
}
```

我们已了解在 Django 项目中结构化代码的重要性以及它能为我们带来的好处。在下一节中，我们将了解如何在 Django 项目中管理异常和错误处理。

# 处理异常和错误

开发人员应始终考虑所有可能出现的边界情况并编写代码来处理所有这些边界情况。不幸的是，错误、异常和 bug 仍可能溜进生产环境，导致用户体验不佳。我们已在 *Chapter 9* 中学习到，测试用例如何帮助我们在开发阶段更早地捕获这些错误，并且为了改善用户体验，我们应在出现问题时显示适当的错误消息。例如，如果服务器期望请求体中存在 `name`，而它缺失，则我们应发送明确的错误消息，说明 `name field is missing`。

以下是我们在处理异常和错误时应遵循的几点重要事项：

*   始终发送适当的响应状态码。如果请求体缺少某些信息，则传递 `400`，然后传递解释缺失信息的消息。

*   如果错误是身份验证错误，则发送 `401`。如果我们使用 DRF，则 DRF 会自动以适当的 HTTP 状态码响应。

*   如果请求错误是授权错误，则发送 `403`。

*   为错误代码创建结构。例如，如果博客应用中的 API 收到错误请求，我们应创建自定义代码，`BLG0023` 或 `BLG0042`，以帮助识别代码的哪一部分出现故障并返回错误响应。在我们的示例中，通过 `BLG0023`，我们可以使用 API 错误代码识别有关服务、模块和功能的更多详细信息。这是行业中最广泛使用的策略之一；例如，你可能已注意到 ATM 上显示此类错误代码。

*   集成错误监控工具，如 Rollbar (`https://rollbar.com/`) 或 Sentry (`https://sentry.io/welcome/`) 以捕获错误。我们将在 *Chapter 14* 中详细讨论集成。

*   在代码中实现异常块时，我们应避免使用宽泛的异常处理并静默忽略错误。有一个详细的 Stack Overflow 回答解释了原因：`https://stackoverflow.com/questions/21553327/`。

让我们举一个如何在代码库中实现更好的错误处理的例子。假设用户请求数据库中不存在的特定博客。我们应按如下方式处理这种情况：

```
@api_view(['GET'])
def get_blog_by_id(blog_id):
    try:
        blog = Blog.objects.get(id=blog_id)
    except Blog.DoesNotExist:
        return Response(
          status=status.HTTP_404_NOT_FOUND,
          data={'error': 'Blog does not exist',
                'error_code': 'BLG0012'}
        )
    blog_data = BlogSerializer(blog).data
    return Response({'blog': blog_data}, status=status.HTTP_200_OK)
```

在这里，我们捕获错误并发送适当的错误消息和错误代码供团队调试，同时附带正确的 HTTP 状态码。

我们在本节学习了错误处理和监控。随着我们在项目中构建新功能，我们希望对功能更有信心，同时在检测到破坏性更改时也有选项立即回滚。这就是功能标志/ kill switch 发挥作用的地方。

# 使用功能标志

功能标志是一种在运行时启用或禁用代码中某些功能的技术，而无需部署新代码。这有助于开发人员以更快的速度在生产环境中切换不同功能。例如，如果一家公司已在其应用中集成 Stripe 和 PayPal 支付网关，但由于某种原因 Stripe 出现故障，则团队可以快速在无需任何代码更改或部署的情况下将支付网关更改为 PayPal。这对于关键系统以及构建新系统时非常常见。





在下面的示例代码中，我们将支付网关配置存储在 `KeyValueStore` 数据库表中。这有助于我们在运行时控制支付网关。`KeyValueStore` 是一个数据库模型，存储在 `common/models.py` 文件中：

```
class KeyValueStore(models.Model):
    key = models.CharField(max_length=255, unique=True)
    value = models.JSONField()
    def __str__(self):
        return self.key
```

现在，创建一个公共接口，供其他应用使用以获取配置值。创建 `common/public.py` 文件并添加以下内容：

```
from common import models
def get_current_config(key_name):
  try:
    return models.KeyValueStore.objects.get(key=key_name).value
  except models.KeyValueStore.DoesNotExist:
    return {}
```

现在，我们可以使用 `common` 配置函数访问存储在 `KeyValueStore` 模型中的任何值：

```
from common import public
def get_current_gateway():
    return public.get_current_config('payment_gateway')
def get_payment_gateway():
    payment_config = get_current_gateway()
    if payment_config.get('name') == 'stripe':
        return 'stripe'
    elif payment_config.get('name') == 'paypal':
        return 'PayPal'
    return 'stripe'
def make_payment(request):
    # do something related to payment
    payment_gateway = get_payment_gateway()
    # process payment
```

功能开关对开发者学习非常重要，因为它让他们对系统更有信心。同样，该策略也有助于开发者使用 `kill switches`，并在不依赖代码重新部署的情况下创建 A/B 测试和发布策略。使用简单的数据库和代码逻辑即可轻松实现基础的功能开关和 kill switches，但随着系统规模的增长，可以考虑使用外部系统来实现此类目的。诸如 Flagsmith（[`flagsmith.com`](https://flagsmith.com)）和 LaunchDarkly（[`launchdarkly.com`](https://launchdarkly.com)）等工具可以帮助构建复杂的功能开关和发布策略。

我们已经了解了功能开关及其在开发周期中的重要性。现在我们将来看一些关于如何为生产环境配置 Django 的技巧。

# 为生产环境配置 Django

在本节中，我们将学习在部署到生产环境之前需要设置的一些关键配置。我们将首先讨论需要更新 `settings.py` 文件的配置：

* 更新 `SECRET_KEY` 值并通过环境变量传递
* 将 `DEBUG = False` 设置为生产环境，以便在 Django 应用出现错误时，不会通过错误堆栈跟踪显示敏感信息
* 设置 `ALLOWED_HOSTS = ["dip.com", "xyz.com"]`，并列出 Django 项目将被部署的所有域名
* 设置 `APPEND_SLASH = True`，以便如果任何请求末尾缺少斜杠，Django 可以自动添加斜杠
* 适当设置 `TIME_ZONE`

还有一些其他配置可以根据 Django 项目的用例进行更新。除前面提到的设置外，让我们学习另外几个需要执行的配置：

* 将 Django 管理路径从 `/admin` 更改为不可猜测的路径
* 对于任何与时间相关的操作，始终使用 `timezone.now()`

在本书中，我们学习了如何创建 REST API —— 除非我们纯粹为移动应用构建，否则我们需要配置 `Cross-Origin Resource Sharing`（`CORS`），这将使我们的 Django 应用可以从其他源访问。

## 设置 CORS

CORS 是一种允许从不同源访问资源的机制。CORS 特别适用于浏览器，而非其他设备，如移动设备或物联网。有一种第三方包 `django-cors-headers`，被广泛用于为 Django 应用配置 CORS。请记住，CORS 和 `Cross-Site Request Forgery`（`CSRF`）是不同的，*Django 仅提供对 CSRF 的支持，而不提供对 CORS 的支持*。

以下是如何在 Django 项目中配置 CORS 的步骤：

1. 通过运行以下命令安装 `django-cors-headers` 包：

```
pip install django-cors-headers
```

2. 将 `corsheaders` 添加到 `INSTALLED_APPS` 配置列表中：

```
INSTALLED_APPS = [
    ...,
    "corsheaders",
    ...,
]
```

3. 在 Django 设置的 `MIDDLEWARE` 配置中添加 `corsheaders.middleware.CorsMiddleware`：

```
MIDDLEWARE = [
    ...,
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.common.CommonMiddleware",
    ...,
]
```

4. 现在，在 `settings.py` 文件的 `CORS_ALLOWED_ORIGINS` 配置中添加你想要启用 CORS 的域名/子域名：

```
CORS_ALLOWED_ORIGINS = [
    "https://example.com",
    "https://sub.example.com",
    "http://127.0.0.1:9000",
]
```

我们已为项目配置了基本 CORS 配置。

注意

还有一些其他配置可以进行，但它们取决于项目的用例。请随时查阅官方文档以了解更多可应用的配置，网址为 [`github.com/adamchainz/django-cors-headers`](https://github.com/adamchainz/django-cors-headers)。

我们需要学习的最后一个概念是将 Django 应用部署到项目中，那就是 `Web Server Gateway Interface`（`WSGI`）。

## 探索 WSGI

WSGI 是 PEP 333 中规定的规范。该规范定义了 Nginx 和 Apache 等 Web 服务器与 Django 和 Flask 等 Python Web 框架/应用之间的标准接口。WSGI 实现的细节超出了本书的范围。我们将只学习如何将 WSGI 集成到 Django 项目中。

Django 创建的样板代码已经生成了 `wsgi.py` 文件。这是生产环境中 Django 项目的入口点。我们需要在 `wsgi.py` 文件中添加配置以设置 `settings.py` 文件，通过在 `DJANGO_SETTINGS_MODULE` 变量中设置 `settings.py` 文件路径。例如，如果我们的设置文件位于 `config/settings/prod.py`，则应将路径设置为 `os.environ["DJANGO_SETTINGS_MODULE"]="backend.settings.prod"`。

配置完 `wsgi.py` 文件后，我们将使用 `gunicorn` 启动项目。请记住，WSGI 是规范，而 `gunicorn` 是用于 UNIX 的 Python WSGI HTTP 服务器。我们不会深入探讨 `gunicorn` 的概念，因为它本身就是一个框架。相反，我们将专注于 `gunicorn` 与 Django 项目的集成。

重要提示

Gunicorn 不支持 Windows 环境。由于我们将使用 Linux 环境部署代码，如果你使用的是 Windows 设置，请跳过此步骤。或者，你可以使用 waitress Python 包在本地进行测试，但这在其他章节中不会使用。更多详情，请查看 *第 10 章* 的 GitHub readme。

以下是将 `gunicorn` 集成到任何 Django 项目的步骤：

1. 我们已经看到了如何配置 `wsgi.py` 文件。你可以根据本节前面提到的说明设置 `wsgi.py`。
2. 现在，我们将在项目中使用以下命令安装 `gunicorn`：

```
pip install gunicorn
```

3. 使用 WSGI 文件启动 `gunicorn` 服务器。确保从 `manage.py` 文件所在的同一路径运行以下命令：

```
gunicorn config.wsgi:application --bind 0.0.0.0:8000 --workers=4 --max-requests=512 --max-requests-jitter=64 
```

代码片段中有很多标志。让我们现在分解每个配置：




`config.wsgi.application` 是我们 Django WSGI 配置的路径。这是生产环境中 Django 应用的入口点。

`--bind 0.0.0.0:8000` 是将应用绑定到 `0.0.0.0:8000` 端口的配置，我们将配置 Nginx 反向代理将流量发送到端口 `8000`。

`--workers=4` 是运行 `gunicorn` 服务器的四个 worker。

`--max-request=512` 是告诉 `gunicorn` 在处理了 512 个请求后重启服务器。这在应用存在内存泄漏时特别有帮助。

`--max-request-jitter=64` 是在重启服务器时引入抖动效果，否则我们可能会看到所有服务器在后台同时重启，导致停机。

Django 还支持使用异步服务器网关接口 (`ASGI`) 进行部署，它为异步 Python Web 服务器、框架和应用之间提供了标准接口规范。ASGI 的实现超出了本书的范围。然而，如果你想实现它，你可以查看官方文档了解更多详细信息，网址为 [`docs.djangoproject.com/en/stable/howto/deployment/asgi/`](https://docs.djangoproject.com/en/stable/howto/deployment/asgi/)。

我们已经涵盖了与 Django 及构建可扩展 Django 项目所需的其他包相关的代码概念。

# 总结

在本章中，我们学习了可以在 Django 项目中实现的不同约定。我们学习了如何在 Django 项目中创建不同的文件并对其进行适当的结构化，以编写可扩展且可维护的代码，并在项目扩展时避免循环依赖错误。异常和错误处理是我们本章学习的重要概念。我们了解了错误代码如何帮助我们在生产环境中正确调试。

我们看到了功能标志如何帮助我们更好地控制代码，并在运行时启用/禁用某些功能，而无需向服务器额外部署代码。然后，我们看到了在将 Django 应用部署到生产环境之前配置应用的一些技巧。CORS 和 WSGI 是 Web 开发的关键规范，我们在本章中学习了它们，以及 `django-cors-headers` 和 `gunicorn` 包。我们还学习了应用开发，以及如何编写可扩展的应用。

在下一部分，我们将开始学习开发中使用的不同工具，如 Docker、Git 等。我们将学习使用版本控制的最佳实践，特别是 Git 和 githooks，以及如何利用持续集成 (`CI`) 流水线，如 GitHub Actions 和 GitLab CI，以获得更好的开发流程。
