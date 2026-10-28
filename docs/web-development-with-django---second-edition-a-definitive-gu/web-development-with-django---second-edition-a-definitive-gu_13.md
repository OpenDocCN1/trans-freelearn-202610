## Django 第三方库

由于 Django 自 2007 年以来一直存在，因此已经形成了丰富的第三方库生态系统，可以将这些库插入应用程序，为其提供额外功能。到目前为止，我们已经学习了许多 Django 知识，并使用了它的许多功能，包括数据库模型、URL 路由、模板、表单等。我们直接使用这些 Django 工具构建了 Web 应用程序，但现在我们将了解如何利用他人的工作，快速为自己的应用程序添加更多高级功能。我们之前提到过用于存储文件的应用程序（在第 5 章“提供静态文件”中，我们提到过一个名为 django-storages 的应用程序，它可以将静态文件存储在 CDN 中），但除了文件存储之外，我们还可以使用应用程序接入第三方身份验证系统、集成支付网关、自定义设置的构建方式、修改图像、更轻松地构建表单、调试网站、使用不同类型的数据库，以及完成更多工作。如果你想添加某项功能，很可能已经存在能够实现该功能的应用程序。

本章无法涵盖所有应用程序，因此我们只关注其中四个应用程序，它们能够为许多不同类型的应用程序提供实用功能。django-configurations 允许你使用类配置 Django 设置，并利用继承简化不同环境的设置。这一功能可以与 dj-database-urls 配合使用，只需通过一个 URL 指定数据库连接设置。Django Debug Toolbar 可以让你直接在浏览器中获取额外信息，以帮助调试。最后要介绍的应用程序是 django-crispy-forms，它提供了额外的 CSS 类，使表单外观更加美观，同时还可以仅使用 Python 代码更轻松地进行配置。

对于这些库中的每一个，我们都将介绍其安装、基本设置和使用方式，主要说明它们如何应用于 Bookr。它们还提供了更多配置选项，可以进一步定制以满足应用程序的需求。这些应用程序都可以使用 pip 安装。

我们还将简要介绍 django-allauth，它允许 Django 应用程序通过第三方提供商（例如 Google、GitHub、Facebook 和 Twitter）对用户进行身份验证。我们不会详细介绍它的安装和设置，但会提供一些示例，帮助你完成配置。

本章将涵盖以下主题：

- 环境变量
- `django-configurations`
- `dj-database-urls`
- Django Debug Toolbar
- `django-crispy-forms`
- `django-allauth`

## 技术要求

本章的所有代码文件都可以在以下地址找到：https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter15。

## 环境变量

创建程序时，我们通常希望用户能够配置程序的某些行为。例如，假设你有一个连接数据库并将找到的所有记录保存到文件中的程序。通常，它可能只会向终端输出一条*成功*消息，但你可能还希望在*调试模式*下运行它，使其同时输出正在执行的所有 SQL 语句。

配置此类程序有很多方法。例如，可以让程序从配置文件中读取设置。但在某些情况下，用户可能希望快速使用特定设置（例如调试模式）运行 Django 服务器，然后再次关闭该设置运行服务器。每次都修改配置文件可能很不方便。在这种情况下，我们可以从*环境变量*中读取设置。环境变量是可以在操作系统中设置、然后由程序读取的键值对。设置环境变量有以下几种方式：

- Shell（终端）可以在启动时从配置文件脚本中读取变量，之后每个程序都可以访问这些变量。
- 可以在终端中设置变量，随后启动的任何程序都可以使用该变量。在 Linux 和 macOS 中，可以使用 `export` 命令完成此操作；Windows 使用 `set` 命令。以这种方式设置的变量会覆盖配置文件脚本中的变量，但只对当前会话有效。关闭终端后，这些变量就会丢失。
- 可以在终端运行命令的同时设置环境变量。这些变量只会在正在运行的程序中持续存在，并且会覆盖已导出的环境变量以及从配置文件脚本中读取的变量。
- 可以在正在运行的程序内部设置环境变量，这些变量只会在该程序内部可用（或者对该程序启动的其他程序可用）。以这种方式设置的环境变量会覆盖我们刚才介绍的所有其他方式设置的变量。





在 Linux 和 macOS 上，我们也可以通过在命令前设置它们来提供环境变量（实际命令只是 `python3 env_example.py`）：

```
$ VAR2="Set From Command Line" VAR3="Also Set From Command Line"
python3 env_example.py
```

**重要说明**
请注意，前面的命令在 Windows 上无法工作。对于 Windows，环境变量必须在执行前设置，不能同时传入。

此前面命令的输出如下：

```
UNSET_VAR:UNSET_VAR_VALUE
SET_VAR:Set Using Export
HOME: /Users/ben
VAR1: Set Using Export
VAR2: Set From Command Line
VAR3: Also Set From Command Line
VAR4:
```

Python 脚本对环境变量的解释如下：

- 当脚本运行 `os.environ.setdefault('UNSET_VAR', 'UNSET_VAR_VALUE')` 时，值在脚本内部设置，因为 shell 未为 UNSET_VAR 设置值。输出的值是脚本本身设置的值。
- 当执行 `os.environ.setdefault('SET_VAR', 'SET_VAR_VALUE')` 时，值未被设置，因为 shell 已提供了一个值。这是通过 `export SET_VAR="Set Using Export"` 命令设置的。
- HOME 的值并未由任何已运行的命令设置——这是 shell 提供的。它是用户的主目录。这只是 shell 通常提供的一个环境变量的示例。
- VAR1 由 export 设置，并在执行脚本时未被覆盖。
- VAR2 由 export 设置，但在执行脚本时随后被覆盖。
- VAR3 仅在执行脚本时设置。
- VAR4 从未被设置——我们使用 get 方法访问它以避免 KeyError。

既然环境变量已经介绍完毕，我们就可以回到讨论需要对 manage.py 进行哪些更改以支持 django-configurations。

## django-configurations

将 Django 应用程序部署到生产环境时，主要考虑之一是如何配置它。正如你在本书中看到的那样，`settings.py` 文件是所有 Django 配置的定义位置。即使是第三方应用，其配置也在此文件中。你已经在 *第 12 章，构建 REST API* 中看到过这一点，在使用 Django REST framework 时。

在 Django 中提供不同的配置并在它们之间切换有很多方法。如果你已经开始在现有应用程序上工作，该应用程序已有特定的方法在开发和生产环境之间切换配置，那么你可能应该继续使用该方法。

当我们在 *第 17 章，部署 Django 应用程序（第 1 部分 - 服务器设置）* 中将 Bookr 发布到产品 Web 服务器时，我们需要切换到生产配置，而这时我们将使用 `django-configurations`。

要安装 `django-configurations`，请使用 `pip3` 如下：

```
pip3 install django-configurations
```

> **重要说明**
> 对于 Windows，你可以使用 `pip` 代替前述命令中的 `pip3`。

输出将如下所示：

```
Collecting django-configurations
  Downloading django_configurations-2.4-py3-none-any.whl
    (17 kB)
Requirement already satisfied: django>=3.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django-configurations) (4.0)
Requirement already satisfied: sqlparse>=0.2.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2->django-configurations) (0.4.2)
Requirement already satisfied: asgiref<4,>=3.4.1 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2->django-configurations) (3.4.1)
Requirement already satisfied: backports.zoneinfo; python_version < "3.9" in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2->django-configurations) (0.2.1)
Installing collected packages: django-configurations
Successfully installed django-configurations-2.4
```

`django-configurations` 会更改你的 `settings.py` 文件，使所有设置都从你定义的一个类读取，该类将是 `configurations.Configuration` 的子类。设置不再是 `settings.py` 中的全局变量，而是你定义的类的属性。通过使用这种基于类的 方法，我们可以利用面向对象的范式，最值得注意的是继承。设置在类中定义，可以从另一个类继承设置。例如，生产设置类可以继承开发设置类，只覆盖一些特定的设置——例如在生产环境中强制 `DEBUG` 为 `False`。

我们可以通过仅显示文件中的前几个设置来说明需要对设置文件做什么。标准的 Django `settings.py` 文件通常像这样开始（注释行已删除）：

```
```python
from pathlib import Path


BASE_DIR = Path(__file__).resolve().parent.parent
SECRET_KEY = "django-insecure-c!...-pz6s^_k6w#eo7&"
DEBUG = True
# The rest of the settings are not shown
```
```

要将设置转换为 `django-configurations`，首先从 `configurations` 导入 `Configuration`。然后，定义一个 `Configuration` 子类。最后，将所有设置缩进到类之下。在 PyCharm 中，这只需选择所有设置并按 *Tab* 将它们全部缩进即可。

完成此操作后，你的 `settings.py` 文件将如下所示：

```
```python
from pathlib import Path
from configurations import Configuration


class Dev(Configuration):
    BASE_DIR = Path(__file__).resolve().parent.parent
    SECRET_KEY = "django-insecure-c!...&xxf*skpgkey6%$ld-pz6s^_k6w#eo7&"
    DEBUG = True
    ...
    # All other settings indented in the same manner
```
```

要拥有不同的配置（不同的设置集合），你只需扩展你的配置类并覆盖应不同的设置。

例如，一个需要在生产环境中覆盖的变量是 `DEBUG`；出于安全和性能原因，它应为 `False`。可以定义一个 `Prod` 类，扩展 `Dev` 并设置 `DEBUG`，如下所示：

```
```python
class Dev(Configuration):
    DEBUG = True
    ...
    # Other settings truncated
```
```

```
class Prod(Dev):
    DEBUG = False
    # no other settings defined since we're only overriding
    DEBUG
```

当然，你也可以覆盖其他生产设置，而不仅仅是 DEBUG。通常，出于安全原因，你还会重新定义 SECRET_KEY 和 ALLOWED_HOSTS，并且为了配置 Django 使用你的生产数据库，你也会设置 DATABASES 值。可以按你选择的任何方式配置任何 Django 设置。

如果你现在尝试执行 runserver（或其他管理命令），你将收到错误，因为 Django 不知道如何找到 settings.py 文件，当设置文件像这样布局时：

```
django.core.exceptions.ImproperlyConfigured: django-configurations settings importer wasn't correctly installed. Please use one of the starter functions to install it as mentioned in the docs: https://django-configurations.readthedocs.io/
```

在它再次开始工作之前，我们需要对 manage.py 文件进行一些更改。

## manage.py 更改

需要在 manage.py 中添加/更改两行以启用 django-configurations。首先，我们需要定义一个默认环境变量，告诉 Django 配置应加载哪个 Configuration 类。

此行应添加到 main() 函数中，以设置 DJANGO_CONFIGURATION 环境变量的默认值：

```
os.environ.setdefault('DJANGO_CONFIGURATION', 'Dev')
```

这将其默认值设置为 Dev——我们定义的类的名称。正如我们在示例脚本中看到的那样，如果此值已定义，则不会被覆盖。这将允许我们使用环境变量在配置之间切换。

第二个更改是将 execute_from_command_line 函数替换为 django-configurations 提供的函数。考虑以下行：

```
from django.core.management import execute_from_command_line
```

此行更改如下：

```
from configurations.management import execute_from_command_line
```





从现在开始，manage.py 将像以前一样工作，只是它在启动时会打印出它正在使用的 Configuration 类（图 15.1）：

```
(bookr) -> bookr python manage.py runserver
django-configurations version 2.4, using configuration Dev
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
December 31, 2022 - 02:18:39
Django version 4.0, using settings 'bookr.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

图 15.1 – django-configurations 使用配置 Dev

在第二行，你可以看到 django-configurations 的输出，它对设置使用 Dev 类。

## Configuration from environment variables

除了使用环境变量在 Configuration 类之间切换外，django-configurations 还允许我们使用环境变量为单个设置提供值。它提供了 Value 类，这些类会自动从环境中读取值。我们可以在未提供值时定义默认值。由于环境变量始终是字符串，因此不同的 Value 类用于将字符串转换为指定类型。

让我们通过几个示例在实践中查看这一点。我们将允许使用环境变量设置 DEBUG、ALLOWED_HOSTS、TIME_ZONE 和 SECRET_KEY，如下所示：

```
from configurations import Configuration, values

class Dev(Configuration):
    DEBUG = values.BooleanValue(True)
    ALLOWED_HOSTS = values.ListValue([])
    TIME_ZONE = values.Value('UTC')
    SECRET_KEY = "django-insecure-c!...-pz6s^_k6w#e0?&
    ...
    # Other settings truncated

class Prod(Dev):
    DEBUG = False
    SECRET_KEY = values.SecretValue()
    # no other settings are present
```

我们将逐一解释这些设置：

- 在 Dev 中，DEBUG 从环境变量读取并转换为布尔值。值 yes、y、true 和 1 变成 True；值 no、n、false 和 0 变成 False。这允许我们在开发机器上关闭 DEBUG 运行，这在某些情况下很有用（例如，测试自定义异常页面而不是 Django 的默认页面）。在 Prod 配置中，我们不希望 DEBUG 意外变为 True，因此我们将其静态设置。
- ALLOWED_HOSTS 在生产环境中是必需的。它是 Django 应该接受请求的域名列表。
- ListValue 类会将逗号分隔的字符串转换为 Python 列表。
  例如，字符串 www.example.com, example.com 被转换为 ["www.example.com", "example.com"]。
- TIME_ZONE 只接受字符串值，因此使用 Value 类设置。此类只是读取环境变量且完全不对其进行转换。
- SECRET_KEY 在 Dev 配置中静态定义；它不能通过环境变量更改。在 Prod 配置中，它使用 SecretValue 设置。这与 Value 类似，因为它只是一个字符串设置；但是，它不允许默认值。如果设置了默认值，则会引发异常。这是为了确保你永远不会将密钥值放入 settings.py 中，因为它可能会被意外共享（例如，上传到 GitHub）。请注意，由于我们在生产环境中不使用 Dev 的 SECRET_KEY，因此我们不在乎它是否泄露。

默认情况下，django-configurations 期望每个环境变量都有 DJANGO_ 前缀。例如，要设置 DEBUG，请使用 DJANGO_DEBUG 环境变量，要设置 ALLOWED_HOSTS，请使用 DJANGO_ALLOWED_HOSTS。

现在我们已经介绍了 django-configurations 以及使项目支持它的所需更改，让我们将其添加到 Bookr 并进行这些更改。在下一个练习中，我们将安装并在 Bookr 中设置 django-configurations。

## Exercise 15.01 – Django configurations setup

在本练习中，我们将使用 pip 安装 django-configurations，然后更新 settings.py 以添加 Dev 和 Prod 配置。然后我们将对 manage.py 进行必要的更改以支持新的配置样式，并测试一切是否仍在正常工作。让我们开始以下步骤：

- 1. 在终端中，确保你已激活 bookr 虚拟环境，然后运行以下命令使用 pip3 安装 django-configurations：

```
pip3 install django-configurations
```

> **Note**
>
> 对于 Windows，你可以在前面的命令中使用 pip 而不是 pip3。

安装过程将运行，你应该有类似 *Figure 15.2* 的输出：

```
[(bookr) → bookr] pip3 install django-configurations
Collecting django-configurations
  Using cached django_configurations-2.4-py3-none-any.whl (17 kB)
Collecting django>=3.2
  Downloading Django-4.1.4-py3-none-any.whl (8.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.1/8.1 MB 6.9 MB/s eta 0:00:00
Collecting sqlparse>=0.2.2
  Using cached sqlparse-0.4.3-py3-none-any.whl (42 kB)
Collecting asgiref<4,>=3.5.2
  Downloading asgiref-3.6.0-py3-none-any.whl (23 kB)
Installing collected packages: sqlparse, asgiref, django, django-configurations
Successfully installed asgiref-3.6.0 django-4.1.4 django-configurations-2.4 sqlparse-0.4.3
```

图 15.2 – 使用 pip 安装 django-configurations

- 2. 在 PyCharm 中，打开 `bookr` 包内的 `settings.py`。在现有的 `os` 导入下方，从 `configurations` 导入 `Configuration` 和 `values`，如下所示：

```
from configurations import Configuration, values
```

- 3. 在导入之后但在第一个设置定义之前（设置 `BASE_DIR` 值的行），添加一个新的 `Configuration` 子类，名为 `Dev`：

```
class Dev(Configuration):
```

- 4. 现在，我们需要移动所有现有设置，使它们成为 Dev 类的属性而不是全局变量。在 PyCharm 中，这只需选择所有设置然后按 Tab 键缩进即可。执行此操作后，你的设置应如下所示：

```
from pathlib import Path
from configurations import Configuration, values

class Dev(Configuration):
    # Build paths inside the project like this: BASE_DIR / 'subdir'.
    BASE_DIR = Path(__file__).resolve().parent.parent

    # Quick-start development settings - unsuitable for production
    # See https://docs.djangoproject.com/en/4.0/howto/deployment/checklist/

    # SECURITY WARNING: keep the secret key used in production secret!
    SECRET_KEY = "django-insecure-cled4dqj0us$i%z&fx#spkgkey%$kzplg-pz6^$__k6w#eo7&"

    # SECURITY WARNING: don't run with debug turned on in production!
    DEBUG = True
```

图 15.3 – 新的 Dev 配置

5. 缩进设置后，我们将更改一些设置以从环境变量读取。首先，将 DEBUG 更改为作为 BooleanValue 读取。它应默认为 True。考虑以下行：

```
DEBUG = True
```

然后，将其更改为：

```
DEBUG = values.BooleanValue(True)
```

这将自动从 DJANGO_DEBUG 环境变量读取 DEBUG 并将其转换为布尔值。如果环境变量未设置，则默认为 True。

6. 此外，将 ALLOWED_HOSTS 转换为从环境变量读取，使用 values.ListValue 类。它应默认为 []（空列表）。考虑以下行：

```
ALLOWED_HOSTS = []
```

将其更改为：

```
ALLOWED_HOSTS = values.ListValue([])
```

ALLOWED_HOSTS 将从 DJANGO_ALLOWED_HOSTS 环境变量读取，并默认为空列表。

7. 到目前为止你所做的一切都涉及在 Dev 类上添加/更改属性。现在，在同一文件的末尾，添加一个继承自 Dev 的 Prod 类。它应定义两个属性，DEBUG = True 和 SECRET_KEY = values.SecretValue()。完成的类应如下所示：

```
class Prod(Dev):
    DEBUG = False
    SECRET_KEY = values.SecretValue()
```

保存 settings.py。

8. 如果我们现在尝试运行任何管理命令，我们将收到 django-configurations 未正确设置的错误。我们需要对 manage.py 进行一些更改使其再次工作。打开 bookr 项目目录中的 manage.py。

考虑以下读取的行：

```
os.environ.setdefault('DJANGO_SETTINGS_MODULE',
    'bookr.settings')
```

在其下方添加这一行：

```
os.environ.setdefault('DJANGO_CONFIGURATION', 'Dev')
```

这将默认配置设置为 Dev 类。它可以通过设置 DJANGO_CONFIGURATION 环境变量（例如，设置为 Prod）来覆盖。





+   9. 在上一步行的下方两行，你应该已经有以下导入语句：
+   ```
+   from django.core.management import execute_from_command_line
+   ```
+   将其更改为以下内容：
+   ```
+   from configurations.management import execute_from_command_line
+   ```
+   这将使 manage.py 脚本使用 Django 配置的 execute_from_command_line 函数，而不是 Django 内置的函数。
+   保存 manage.py。

+   10. 启动 Django 开发服务器。如果它能够无错误地启动，你可以确信所做的更改已生效。为了确定，请检查页面是否在浏览器中加载。打开 http://127.0.0.1:8000/ 并尝试浏览网站。所有内容应看起来并感觉与之前一样：

![](img/4840a43efd335abd10b93708225692d6_654_0.png)

在本练习中，我们安装了 django-configurations 并重构了 settings.py 文件以使用其 Configuration 类来定义我们的设置。我们添加了 Dev 和 Prod 配置，并使 DEBUG、ALLOWED_HOSTS 和 SECRET_KEY 能够通过环境变量设置。最后，我们更新了 manage.py 以使用 Django 配置的 execute_from_command_line 函数，这使得使用这种新的 settings.py 格式成为可能。

在下一节中，我们将介绍 dj-database-url，这是一个使你能够使用 URL 配置 Django 数据库设置的包。

输出如下：

```
Collecting dj-database-url
    Downloading https://files.pythonhosted.org/packages/d4/a6/4b8578c1848690d0c307c7c0596af2077536c9ef2a04d42b00fabaa7e49d/dj_database_url-0.5.0-py2.py3-none-any.whl
Installing collected packages: dj-database-url
Successfully installed dj-database-url-0.5.0
```

现在，dj_database_url 可以导入到 settings.py 中，并且 dj_database_url.parse 方法可用于将 URL 转换为 Django 可用的字典。我们可以使用其返回值来设置 DATABASES 字典中的默认值（或其他项）：

```
import dj_database_url

DATABASES = {'default': dj_database_url.parse(
'postgres://bookr:b00ks@db.example.com:5432/bookr_django'
)}
```

或者，对于我们的 SQLite 数据库，我们可以利用 BASE_DIR 设置，就像我们已经在做的那样，并将其包含在 URL 中：

```
import dj_database_url

DATABASES = {'default': dj_database_url.parse(
    f'sqlite:///{BASE_DIR}/db.sqlite3')}
```

解析后，DATABASES 字典与我们之前定义的内容相似。它包含一些不适用于 SQLite 数据库的冗余项（USER、PASSWORD、HOST 等），但 Django 会忽略它们：

```
DATABASES = {
    'default': {
        'NAME': '/Users/ben/bookr/bookr/db.sqlite3',
        'USER': '',
        'PASSWORD': '',
        'HOST': '',
        'PORT': '',
        'CONN_MAX_AGE': 0,
        'ENGINE': 'django.db.backends.sqlite3'
    }
}
```

这种设置数据库连接信息的方法并不太有用，因为我们仍然在 settings.py 中静态定义数据。唯一的区别是我们使用 URL 而不是字典。`dj-database-url` 还可以自动从环境变量读取 URL。这将允许我们通过在环境中设置它们来覆盖这些值。

要从环境中读取数据，请使用 `dj_database_url.config` 函数，如下所示：

```
```python
import dj_database_url

DATABASES = {'default': dj_database_url.config()}
```
```

URL 会自动从 `DATABASE_URL` 环境变量中读取。

我们可以通过向 config 函数也提供一个 `default` 参数来改进这一点。这是如果环境变量中未指定则将使用的默认 URL：

```
```python
import dj_database_url

DATABASES = {
    'default': dj_database_url.config(
        default=f'sqlite:///{BASE_DIR}/db.sqlite3')}
```
```

这样，我们可以指定一个默认 URL，该 URL 可以被生产环境中的环境变量覆盖。

我们还可以通过传入 `env` 参数来指定读取 URL 的环境变量——这是第一个位置参数。这样，你就可以为不同的数据库设置读取多个 URL：

```
```python
import dj_database_url

DATABASES = {
    'default': dj_database_url.config(
        default=f'sqlite:///{BASE_DIR}/db.sqlite3'),
    'secondary': dj_database_url.config(
        'DATABASE_URL_SECONDARY',
        default=f'sqlite:///{BASE_DIR}/db-secondary.sqlite3')
}
```
```

在此示例中，`default` 项的 URL 从 `DATABASE_URL` 环境变量读取，而 `secondary` 从 `DATABASE_URL_SECONDARY` 读取。

django-configurations 还提供了一个与 dj_database_url 协同工作的配置类：DatabaseURLValue。它与 dj_database_url.config 略有不同，因为它生成整个 DATABASES 字典，包括 default 项。例如，考虑以下代码：

```
import dj_database_url

DATABASES = {'default': dj_database_url.config()}
```

此代码等同于以下内容：

```
from configurations import values

DATABASES = values.DatabaseURLValue()
```

不要编写 DATABASES['default'] = values.DatabaseURLValue()，否则你的字典会双重嵌套。

如果需要指定多个数据库，你将需要直接回退到 dj_database_url.config，而不是使用 DatabaseURLValue。

像其他 values 类一样，DatabaseURLValue 将默认值作为其第一个参数。你可能还想使用 environ_prefix 参数并将其设置为 DJANGO，以便读取的环境变量与其他变量命名一致。使用 DatabaseURLValue 的完整示例因此如下所示：

```
DATABASES = values.DatabaseURLValue(
    f'sqlite:///{BASE_DIR}/db.sqlite3',
    environ_prefix='DJANGO')
```

通过这样设置 environ_prefix，我们可以使用 DJANGO_DATABASE_URL 环境变量设置数据库 URL（而不是仅 DATABASE_URL）。这意味着它与其他以 DJANGO_ 开头的环境变量设置（如 DJANGO_DEBUG 或 DJANGO_ALLOWED_HOSTS）保持一致。

请注意，即使我们没有在 settings.py 中导入 dj-database-url，django-configurations 在内部使用它，因此它仍然必须安装。

在下一个练习中，我们将配置 Bookr 使用 DatabaseURLValue 来设置其数据库配置。它将能够从环境变量读取并回退到我们指定的默认值。

## 练习 15.02 – dj-database-url 和设置

在本练习中，我们将使用 pip3 安装 dj-database-url。然后，我们将更新 Bookr 的 settings.py 以使用从环境变量读取的 URL 来配置 DATABASE 设置。让我们开始以下步骤：

+   1. 在终端中，确保你已激活 bookr 虚拟环境，然后运行以下命令使用 pip3 安装 dj-database-url：

```
pip3 install dj-database-url
```

安装过程将运行，你应该有类似以下的输出：

```
((bookr) → bookr pip install dj-database-url
Collecting dj-database-url
Using cached dj_database_url-1.2.0-py3-none-any.whl (7.1 kB)
Requirement already satisfied: dj-database-url==3.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from dj-database-url) (4.0)
Requirement already satisfied: backports.zoneinfo; python_version < "3.9" in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from Django>=3.2->dj-database-url) (0.2.1)
Requirement already satisfied: asgiref>=3.4.1 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from Django>=3.2->dj-database-url) (3.4.1)
Requirement already satisfied: sqlparse>=0.2.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from Django>=3.2->dj-database-url) (0.4.2)
Installing collected packages: dj-database-url
Successfully installed dj-database-url-1.2.0
```

图 15.5 – 使用 pip 安装 dj-database-url

+   2. 在 PyCharm 中，打开 bookr 包目录中的 settings.py。向下滚动找到 DATABASES 属性的定义位置。用 values.DatabaseURLValue 类替换它。第一个参数（默认值）应为 SQLite 数据库的 URL：f'sqlite:///{BASE_DIR}/db.sqlite3'。同时传入 environ_prefix，设置为 DJANGO。完成此步骤后，你应像这样设置该属性：

```
DATABASES = values.DatabaseURLValue(
    f'sqlite:///{BASE_DIR}/db.sqlite3',
    environ_prefix='DJANGO'
)
```

保存 settings.py。

+   3. 启动 Django 开发服务器。与练习 15.01 – Django configurations 设置一样，如果它能正常启动，你可以确信更改成功。为了确定，请在浏览器中打开 http://127.0.0.1:8000/ 并检查一切看起来和行为是否与之前一样。你应该访问一个从数据库查询的页面（例如 Books List 页面）并检查书籍列表是否显示：


![](img/4840a43efd335abd10b93708225692d6_660_0.png)

图15.6 – Bookr页面，数据库查询仍然正常工作

在本练习中，我们更新了settings.py，使其根据环境变量中指定的URL来确定DATABASES设置。我们使用values.DatabaseURLValue类自动读取该值，并提供了默认URL。我们还将environ_prefix参数设置为DJANGO，这样环境变量名称就是DJANGO_DATABASE_URL，与其他设置保持一致。

在下一节中，我们将介绍Django调试工具栏，这是一个帮助你在浏览器中调试Django应用的应用程序。

## Django调试工具栏

Django调试工具栏是一个直接在浏览器中显示网页调试信息的应用程序。它包含生成页面时运行了哪些SQL命令、请求和响应头、页面渲染耗时等信息。这些信息在以下情况下非常有用：

- 页面加载耗时过长——也许是因为运行了太多数据库查询。你可以查看相同的查询是否被多次执行，如果是，可以考虑使用缓存。否则，可以通过为数据库添加索引来加速某些查询。
- 你想确定页面返回错误信息的原因。你的浏览器可能发送了你意料之外的请求头，或者Django返回的某些响应头不正确。
- 页面速度慢是因为在非数据库代码上花费了时间——你可以对页面进行性能分析，查看哪些函数耗时最长。
- 页面显示不正确。你可以查看Django渲染了哪些模板。可能有第三方模板被意外渲染。你还可以检查所有正在使用的设置（包括我们未设置的内置Django设置）。这有助于定位可能导致页面行为异常的设置。

我们将解释如何使用Django调试工具栏来查看这些信息。在深入介绍如何设置和使用Django调试工具栏之前，我们先快速了解一下它。该工具栏显示在浏览器窗口右侧，可以展开和收起以显示信息：

![](img/4840a43efd335abd10b93708225692d6_661_0.png)

图15.7 – Django调试工具栏（收起状态）

上图显示了收起状态的Django调试工具栏。请注意窗口右上角的切换条。点击该工具栏即可展开：

![](img/4840a43efd335abd10b93708225692d6_662_0.png)

*图15.8* 显示了展开后的Django调试工具栏。

安装Django调试工具栏使用`pip`：

```
pip3 install django-debug-toolbar
```

> **注意**
>
> 对于Windows，你可以在上述命令中使用pip代替pip3。

然后，需要进行几个步骤来完成设置，主要是在settings.py中做一些修改：

1. 将debug_toolbar添加到INSTALLED_APPS设置列表中。
2. 将debug_toolbar.middleware.DebugToolbarMiddleware添加到MIDDLEWARE设置列表中。应尽早添加；对于Bookr，可以将其放在列表的第一项。所有请求和响应都会经过这个中间件。
3. 将'127.0.0.1'添加到INTERNAL_IPS设置列表中（可能需要创建此设置）。Django调试工具栏只会显示此处列出的IP地址。
4. 将Django调试工具栏的URL添加到基础的urls.py文件中。我们只想在DEBUG模式下添加这个映射：

```
path('__debug__', include(debug_toolbar.urls))
```

在下一个练习中，我们将详细完成这些步骤。

一旦Django调试工具栏安装并设置完成，你访问的任何页面都会显示DjDT侧边栏（你可以通过DjDT菜单打开或关闭它）。当它打开时，你可以看到另一组可点击的部分，以获取更多信息。

每个面板旁边都有一个复选框，允许你启用或禁用该指标的收集。每收集一个指标都会稍微减慢页面加载速度（尽管通常不明显）。如果你发现某个指标收集速度较慢，可以在此处将其关闭。接下来，我们将逐一介绍该工具栏的每个面板：

- 第一个是History（历史记录），显示浏览器中发出的请求历史。你可以切换到之前的请求，以在Django调试工具栏中查看与之关联的统计信息（图15.9）：

  ![](img/4840a43efd335abd10b93708225692d6_663_0.png)

  图15.9 – DjDT History（历史记录）面板（为简洁起见，截图已裁剪）

- 系统时间和用户时间之间的差异超出了本书的范围；基本上，系统时间是花在内核中的时间（例如，进行网络或文件读写），而用户时间是操作系统内核之外的代码（包括你在Django、Python等中编写的代码）。

  同时还显示了浏览器中花费的时间，例如发出请求所花费的时间以及渲染页面所花费的时间。

- 第四个面板**Settings**（设置）显示你的应用程序正在使用的所有设置（*图15.12*）：

  ![](img/4840a43efd335abd10b93708225692d6_665_0.png)

  图15.12 – DjDT Settings（设置）面板

- 第六个面板**Request**（请求）显示生成响应的视图，以及调用它时使用的参数和关键字参数（`args`和`kwargs`参数）（*图15.14*）。你还可以看到URL映射中使用的URL名称：

  ![](img/4840a43efd335abd10b93708225692d6_667_0.png)

  图15.14 – DjDT Request（请求）面板（为简洁起见，部分面板未显示）

  它还显示请求的cookies、会话中存储的信息（会话在第8章“媒体服务与文件上传”中介绍），以及`request.GET`和`request.POST`数据。

- 第七个面板**SQL**显示构建响应时执行的所有SQL数据库查询（*图15.15*）：

  ![](img/4840a43efd335abd10b93708225692d6_668_0.png)

  图15.15 – DjDT SQL面板

  你可以查看每个查询的执行耗时以及它们的执行顺序。它还会标记相似和重复的查询，以便你重构代码来消除它们。

  每个SELECT查询都会显示两个操作按钮：Sel（select的缩写）和Expl（explain的缩写）。这些按钮不会出现在INSERT、UPDATE或DELETE查询中。

  Sel按钮显示已执行的SELECT语句以及为该查询检索的所有数据（图15.16）：

  ![](img/4840a43efd335abd10b93708225692d6_669_0.png)

  图15.16 – DjDT SQL selected（SQL选择）面板

  Expl按钮显示SELECT查询的EXPLAIN查询（图15.17）：

- ![](img/4840a43efd335abd10b93708225692d6_671_0.png)

  图15.18 – DjDT Static files（静态文件）面板

- 第九个面板Templates（模板）显示渲染过的模板的信息（图15.19）：

  ![](img/4840a43efd335abd10b93708225692d6_672_0.png)

  图15.19 – DjDT Templates（模板）面板

  它显示模板加载的路径和继承链。

- 第10个面板Cache（缓存）显示从Django缓存中获取数据的信息：

  ![](img/4840a43efd335abd10b93708225692d6_673_0.png)

  图15.20 – DjDT Cache（缓存）面板（为简洁起见，部分面板未显示）

  由于我们在Bookr中未使用缓存，此部分为空。如果使用了缓存，我们就能看到对缓存发出了多少次请求，以及其中有多少次成功检索到条目。我们还会看到有多少条目被添加到了缓存中。这可以让你了解缓存使用是否高效。如果你向缓存中添加了大量条目却没有检索到任何条目，那么你应该重新考虑正在缓存的数据。相反，如果缓存未命中次数很多（未命中是指请求的数据不在缓存中），那么你应该缓存比现有更多的数据。





- 第12个面板，**日志**，显示了你的Django应用生成的日志消息（*图15.22*）：

![](img/4840a43efd335abd10b93708225692d6_675_0.png)

*图15.22 — DjDT日志面板*

由于这个请求没有生成任何日志消息，该面板为空。

下一个选项，**拦截重定向**，不是一个包含数据的部分。相反，它允许你切换重定向拦截。如果你的视图返回一个重定向，它不会被跟随。相反，会显示一个类似*图15.22*的页面：

![](img/4840a43efd335abd10b93708225692d6_675_1.png)

*图15.23 — DjDT拦截的重定向*

这允许你为生成重定向的视图打开Django调试工具栏；否则，你只能看到被重定向到的视图的信息。

- 最后一个面板是**性能分析**。默认情况下它是关闭的，因为性能分析可能会大大拖慢你的响应。一旦打开，你必须刷新页面来生成性能分析信息（如图15.24所示）：

![](img/4840a43efd335abd10b93708225692d6_676_0.png)

图15.24 – DjDT性能分析面板

这里显示的信息是响应中每个函数调用所耗时间的分解。页面左侧显示了所有执行的调用的堆栈跟踪。右侧是包含计时数据的列。各列如下：

- **CumTime**：在函数及其调用的任何子函数中花费的累计时间
- **Per**：累计时间除以调用次数（Count）
- **TotTime**：在此函数中花费的时间，但不包括其调用的任何子函数
- **第二个Per**：总时间除以调用次数（Count）
- **Count**：此函数的调用次数

这些信息可以帮助你确定如何加速应用。例如，将一个被调用1,000次的函数略微加速，比优化一个只被调用一次的大型函数要容易得多。关于如何加速代码的更深入技巧超出了本书的范围。

我们将通过以下练习安装和配置Django调试工具栏，来巩固所学的内容。

## 练习15.03 – 设置Django调试工具栏

在本练习中，我们将通过修改`INSTALLED_APPS`、`MIDDLEWARE`和`INTERNAL_IPS`设置来添加Django调试工具栏的设置。然后，我们将把`debug_toolbar.urls`映射添加到bookr包的`urls.py`文件中。最后，我们将在浏览器中加载一个带有Django调试工具栏的页面并使用它：

- 1. 在终端中，确保你已经激活了bookr虚拟环境，然后运行以下命令来使用pip3安装Django调试工具栏：

```
```bash
pip3 install django-debug-toolbar
```
```

> **注意**
>
> 在Windows上，你可以使用`pip`代替前面命令中的`pip3`。

安装过程将运行，你应该会看到类似于图15.25的输出：

```
(bookr) → bookr pip3 install django-debug-toolbar
Looking in indexes: https://pypi.org/simple, https://token:****@gitlab.com/api/v4/projects/28933584/packages/pypi/simple, https://token:****@gitlab.com/api/v4/projects/20903871/packages/pypi/simple
Collecting django-debug-toolbar
  Downloading django_debug_toolbar-3.8.1-py3-none-any.whl (221 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 221 kB 250 kB/s
Requirement already satisfied: sqlparse>=0.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django-debug-toolbar) (0.4.2)
Requirement already satisfied: django>=3.2.4 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django-debug-toolbar) (4.0)
Requirement already satisfied: asgiref<4,>=3.4.1 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2.4->django-debug-toolbar) (3.4.1)
Requirement already satisfied: backports.zoneinfo; python_version < "3.9" in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from django>=3.2.4->django-debug-toolbar) (0.2.1)
Installing collected packages: django-debug-toolbar
Successfully installed django-debug-toolbar-3.8.1
```

图15.25 – 使用pip安装django-debug-toolbar

- 2. 打开bookr包目录中的settings.py。将debug_toolbar添加到INSTALLED_APPS设置中：

```
INSTALLED_APPS = [
    'debug_toolbar',
]
```

这将允许Django找到Django调试工具栏的静态文件。

- 3. 将debug_toolbar.middleware.DebugToolbarMiddleware添加到MIDDLEWARE设置中——它应该是列表中的第一项：

```
MIDDLEWARE = [
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    ...
]
```

这将使请求和响应通过DebugToolbarMiddleware进行路由，允许Django调试工具栏检查请求并将HTML插入响应中。

- 4. 要添加的最后一个设置是将127.0.0.1地址添加到INTERNAL_IPS。你目前还没有定义INTERNAL_IPS设置，因此请将其添加为一项设置：

```
INTERNAL_IPS = ['127.0.0.1']
```

这将使Django调试工具栏只在开发者的计算机上显示。现在你可以保存settings.py了。

- 5. 现在我们需要添加Django调试工具栏的URL。打开bookr包目录中的urls.py。我们已经有一个检查DEBUG模式然后添加媒体URL的if条件，如下所示：

```
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL,
                          document_root=settings.MEDIA_ROOT)
```

我们还将在该if语句中添加对debug_toolbar.urls的include；不过，我们会将其添加到urlpatterns的开头，而不是追加到末尾。请在if语句内添加以下代码：

```
import debug_toolbar

urlpatterns = [
    path('__debug__/',
        include(debug_toolbar.urls)),
] + urlpatterns
```

保存urls.py。

6. 如果Django开发服务器尚未运行，请启动它并导航到http://127.0.0.1:8000。你应该会看到Django调试工具栏打开。如果它没有打开，请点击右上角的DjDT切换按钮将其打开：

![](img/4840a43efd335abd10b93708225692d6_679_0.png)

图15.26 – 角落中显示的DjDT切换按钮

7. 尝试浏览一些面板并访问不同的页面，看看你能找到哪些信息。也请尝试打开“拦截重定向”，然后创建一条新的书评。提交表单后，你应该会看到被拦截的页面，而不是被重定向到新书评（图15.27）：

![](img/4840a43efd335abd10b93708225692d6_679_1.png)

图15.27 – 提交新书评后的重定向拦截页面

然后你可以点击**Location**链接转到它原本要重定向到的页面。

- 8. 你也可以尝试打开**性能分析**，查看哪些函数被大量调用，哪些函数占用了大部分渲染时间。
- 9. 一旦你完成了对Django调试工具栏的实验，请关闭**拦截重定向**和**性能分析**。

在本练习中，我们通过添加设置和URL映射，安装并设置了Django调试工具栏。然后我们看到了它的实际效果，并检查了它能提供给我们的有用信息，包括如何处理重定向以及查看性能分析信息。

在下一节中，我们将学习`django-crispy-forms`应用，它可以减少我们编写表单所需的代码量。

## django-crispy-forms

在Bookr中，我们使用Bootstrap CSS框架。它提供了可以通过CSS类应用于表单的样式。由于Django独立于Bootstrap，当我们使用Django表单时，它甚至不知道我们在使用Bootstrap，因此不知道应该将哪些类应用于表单组件。

`django-crispy-forms`充当Django表单和Bootstrap表单之间的中介。它可以获取一个Django表单，并使用正确的Bootstrap元素和类进行渲染。它不仅支持Bootstrap，还支持其他框架，如**Uni-Form**和**Foundation**（不过Foundation支持必须使用单独的包`crispy-forms-foundation`来添加）。

它的安装和设置非常简单。同样，它使用`pip3`安装：

```
pip3 install django-crispy-forms
```

> **注意**
> 在Windows上，你可以使用`pip`代替前面命令中的`pip3`。

然后，只需进行几项设置更改。首先，将`crispy_forms`添加到你的`INSTALLED_APPS`中。然后，你需要告诉`django-crispy-forms`你正在使用什么框架，以便它加载正确的模板。这通过`CRISPY_TEMPLATE_PACK`设置来完成。在我们的案例中，它应该设置为`bootstrap4`：

```
CRISPY_TEMPLATE_PACK = 'bootstrap4'
```

`django-crispy-forms`有两种主要操作模式：作为过滤器或作为模板标签。前者更容易插入到现有模板中。后者允许更多配置选项，并将更多HTML生成移入`Form`类。我们将依次介绍这两种模式。





## The crispy filter

The first method to render a form with django-crispy-forms is by using the crispy template. First, the filter must be loaded into the template. The library name is crispy_forms_tags:

```
{% load crispy_forms_tags %}
```

Then, instead of rendering a form with the as_p method (or another method), use the crispy filter. Consider the following line:

```
{{ form.as_p }}
```

Replace it with this:

```
{{ form|crispy }}
```

Here's a quick before-and-after showing the Review Create form. None of the rest of the HTML has been changed, apart from the form rendering. Figure 15.28 shows the standard Django form:

![](img/4840a43efd335abd10b93708225692d6_681_0.png)

Figure 15.28 – The Review Create form with default styling

Figure 15.29 shows the form after django-crispy-forms has added the Bootstrap classes:

![](img/4840a43efd335abd10b93708225692d6_682_0.png)

Figure 15.29 – The Review Create form with Bootstrap classes added by django-crispy-forms

When we integrate django-crispy-forms into Bookr, we will not use this method; however, it is worth knowing about because of how easy it is to drop it into your existing templates.

## The crispy template Tag

The other method of rendering a form with django-crispy-forms is with the use of the crispy template tag. To use it, the crispy_forms_tags library must first be loaded into the template (as we did in the previous section). Then, the form is rendered like this:

```
{% crispy form %}
```

How does this differ from the crispy filter? The crispy template tag will also render the <form> element and the {% csrf_token %} template tag for you. So, consider, for example, that you used it like this:

```
<form method="post">
  {% csrf_token %}
  {% crispy form %}
</form>
```

The output for this would be as follows:

```
<form method="post" >
<input type="hidden" name="csrfmiddlewaretoken" value="...">
<form method="post">
<input type="hidden" name="csrfmiddlewaretoken" value="...">
    ... form fields ...
</form>
</form>
```

That is, the form and CSRF token fields are duplicated. In order to customize the <form> element that is generated, django-crispy-forms provides a FormHelper class, which can be set as a Form instance's helper attribute. It is the FormHelper instance that the crispy template tag uses to determine what attributes <form> should have.

Let us look at ExampleForm with a helper added. First, import the required modules:

```
from django import forms
from crispy_forms.helper import FormHelper
```

Next, define a form:

```
class ExampleForm(forms.Form):
    example_field = forms.CharField()
```

We could instantiate a FormHelper instance and then set it to the form.helper attribute (for example, in a view), but it's usually more useful to just create and assign it inside the form's __init__ method. We haven't created a form with an __init__ method yet, but it's no different from any other Python class:

```
def __init__(self, *args, **kwargs):
    super().__init__(*args, **kwargs)
```

Next, we set the helper and form_method for the helper (which is then rendered in the form's HTML):

```
self.helper = FormHelper()
self.helper.form_method = 'post'
```

Other attributes can be set on the helper, such as form_action, form_id, and form_class. We don't need to use these in Bookr though. We also do not need to manually set enctype on the form or its helper, as the crispy form tag will automatically set this to multipart/form-data if the form contains file upload fields.

If we tried to render the form now, we wouldn’t be able to submit it, as there’s no submit button (remember that we added submit buttons to our forms manually; they are not part of the Django form). django-crispy-forms also includes layout helpers that can be added to the form. They will be rendered after the other fields. We can add a submit button like this – first, import the Submit class:

```
from crispy_forms.layout import Submit
```

> **Note**
> django-crispy-forms does not properly support using a <button> input to submit a form, but for our purposes, <input type="submit"> is functionally identical.

We then instantiate it and add it to the helper’s inputs in a single line:

```
self.helper.add_input(Submit("submit", "Submit"))
```

The first argument to the Submit constructor is its name, and the second is its label.

django-crispy-forms is aware that we are using Bootstrap and will automatically render the button with the btn btn-primary classes.

The advantage of using a crispy template tag and FormHelper is that it means there is only one place where attributes and the behavior of the form are defined. We are already defining all the form fields in a Form class; this allows us to define the other attributes of the form in the same place. We can change a form from a GET submission to a POST submission easily here. The FormHelper instance will then automatically know that it needs to add a CSRF token to its HTML output when rendered.

We’ll put all this into practice in the next exercise, where you will install django-crispy-forms, update SearchForm to utilize a form helper, and then render it using the crispy template tag.

## Exercise 15.04 – using Django Crispy Forms with SearchForm

In this exercise, you will install django-crispy-forms and then convert SearchForm to be usable with the crispy template tag. This will be done by adding an __init__ method and building a FormHelper instance inside it:

+   1. In a terminal, make sure you have activated the bookr virtual environment, and then run this command to install django-crispy-forms using pip3:

```
pip3 install django-crispy-forms
```

> **Note**
> For Windows, you can use pip instead of pip3 in the preceding command.

+   6. Still inside the `__init__` method, set `self.helper` to an instance of `FormHelper`. Then, set the helper’s `form_method` to `get`. Finally, create an instance of `Submit`, passing in an empty string as the name (first argument), and `Search` as the button label (second argument). Add this to the helper with the `add_input` method:

```
self.helper = FormHelper()
self.helper.form_method = "get"
self.helper.add_input(Submit("", "Search"))
```

You can save and close `forms.py`.

+   7. In the reviews app’s templates directory, open `search-results.html`. At the start of the file, after the `extends` template tag, use a `load` template tag to load `crispy_forms_tags`:

```
{% load crispy_forms_tags %}
```

+   8. Locate the existing `<form>` in the template. It should look like this:

```
<form>
    {{ form.as_p }}
    <button type="submit" class="btn btn-primary">
        Search</button>
</form>
```

You can delete the entered `<form>` element and replace it with a crispy template tag:

```
{% crispy form %}
```

This will use the `django-crispy-forms` library to render the form, including the `<form>` element and submit button. After making this change, this portion of the template should look like *Figure 15.31*:

![](img/4840a43efd335abd10b93708225692d6_686_0.png)

Figure 15.31 – search-results.html after replacing `<form>` with the crispy form renderer

You can now save `search-results.html`.

9. Start the Django dev server if it is not already running and go to http://127.0.0.1:8000/book-search/. You should see the book search form as shown in Figure 15.32:

![](img/4840a43efd335abd10b93708225692d6_687_0.png)

Figure 15.32 – The book search form rendered with django-crispy-forms

You should be able to use the form in the same manner as you did before (Figure 15.33):

![](img/4840a43efd335abd10b93708225692d6_688_0.png)

Figure 15.33 – Performing a search with the updated search form

Try viewing the source of the page in your web browser to see the rendered output. You will see that the <form> element has been rendered with the method="get" attribute, as we specified to FormHelper in step 5. Note also that django-crispy-forms has not inserted a CSRF token field – it knows that one is not required for a form submitted using GET.

In this exercise, we installed django-crispy-forms using pip3 (pip for Windows) and then configured it in settings.py by adding it to INSTALLED_APPS and defining CRISPY_TEMPLATE_PACK we wanted to use (in our case, bootstrap4). We then updated the SearchForm class to use a FormHelper instance to control the attributes on the form and added a submit button using the Submit class. Finally, we changed the search-results.html template to use the crispy template tag to render the form, which allowed us to remove the <form> element we were using before and simplify form generation by moving all the form-related code into Python code (instead of it being partially in HTML and partially in Python).




除了让 Django 网站具有现代外观外，我们还需要一种方式为我们的网站提供既方便又受用户信任的认证标准。

## django-allauth

浏览网站时，你可能见过一些按钮，允许你使用另一个网站的凭据登录——例如，使用你的 GitHub 账号登录：

![](img/4840a43efd335abd10b93708225692d6_689_0.png)

图 15.34 – 提供使用 Google 或 GitHub 登录选项的登录表单

在解释这一过程之前，先介绍我们将使用的术语：

- **请求站点**：用户试图登录的站点。
- **认证提供商**：用户进行认证的第三方提供商（例如 Google 或 GitHub）。
- **认证应用：**由请求站点的创建者在认证提供商处设置的应用。它决定了请求站点将从认证提供商处获得哪些权限。例如，请求应用可以获取你的 GitHub 用户名，但没有权限写入你的仓库。用户可以通过禁用认证应用的访问权限，来阻止请求站点从认证提供商处获取你的信息。

无论你选择哪种第三方登录选项，该过程通常都是相同的。首先，你将被重定向到认证提供商网站，并被要求授权认证应用访问你的账户（图 15.35）：

![](img/4840a43efd335abd10b93708225692d6_690_0.png)

图 15.35 – 认证提供商授权屏幕

在授权认证应用之后，认证提供商会将你重定向回请求站点。你被重定向到的 URL 将包含一个秘密令牌，请求站点可以据此在后端请求你的用户信息。这使请求站点能够直接与认证提供商通信，以验证你的身份。使用令牌验证你的身份后，请求站点可以将你重定向到你的内容。该流程如图 15.36 中的时序图所示：

![](img/4840a43efd335abd10b93708225692d6_691_0.png)

图 15.36 – 第三方认证流程

现在我们已经介绍了如何使用第三方服务进行认证，接下来可以讨论 `django-allauth` 了。这个应用可以轻松地将你的 Django 应用接入第三方认证服务，包括 Google、GitHub、Facebook 和 Twitter。事实上，在撰写本文时，`django-allauth` 已支持超过 75 个认证提供商。

当用户首次在你的站点进行认证时，`django-allauth` 会为你创建一个标准的 Django User 实例。它还知道如何解析最终用户授权认证应用后认证提供商加载的回调/重定向 URL。

`django-allauth` 会向你的应用添加三个模型：

- `SocialApplication`：用于存储标识你的认证应用所需的信息。你输入的信息取决于提供商，提供商将为你提供*客户端 ID*、*密钥*以及（可选）一个*键*。请注意，这些是 `django-allauth` 对这些值使用的名称，并且会因提供商而异。稍后我们会在本节中给出这些值的一些示例。`SocialApplication` 是你唯一需要自行创建的 `django-allauth` 模型；当用户认证时，`django-allauth` 会自动创建其他模型。
- `SocialApplicationToken`：包含向认证提供商标识 Django 用户所需的值。它包含一个*令牌*以及（可选）一个*令牌密钥*。它还包含对创建它的 `SocialApplication` 模型的引用，以及它所适用的 `SocialAccount` 模型的引用。
- `SocialAccount`：将 Django 用户与提供商（例如 Google 或 GitHub）关联，并存储提供商可能提供的额外信息。

由于认证提供商非常多，我们不会逐一介绍如何设置它们，但会提供简短的设置说明，以及如何将来自提供商的认证令牌映射到 `SocialApplication` 中的正确字段。我们将以本章中一直提到的两个认证提供商——Google 和 GitHub——为例进行说明。

## django-allauth 的安装与设置

与本章中的其他应用一样，`django-allauth` 使用 pip3 安装：

```
```bash
pip3 install django-allauth
```
```

> **注意**
> 
> 在 Windows 上，你可以使用 `pip` 代替上述命令中的 `pip3`。

然后我们需要做一些设置更改。`django-allauth` 需要 `django.contrib.sites` 应用才能运行，因此需要将其添加到 `INSTALLED_APPS` 中。然后，需要添加一个新设置，为我们的站点定义 `SITE_ID` 值。我们可以在 `settings.py` 文件中将其设置为 1：

```
```python
INSTALLED_APPS = [
    "bookr_admin.apps.BookrAdminConfig",
    "django.contrib.sites",  # this entry added
    "django.contrib.auth",
    # The rest of the values are truncated
]

SITE_ID = 1
```
```

> **注意**
> 
> 一个 Django 项目可以托管在多个主机名上，并在每个主机名上有不同的行为，同时也可以在所有站点之间共享内容。我们在项目中的其他地方不需要使用 `SITE_ID`，但必须在此处设置一个实例。你可以在 https://docs.djangoproject.com/en/3.0/ref/contrib/sites/ 阅读更多关于 `SITE_ID` 设置的信息。

我们还需要将 `allauth` 和 `allauth.socialaccount` 添加到 `INSTALLED_APPS`：

```
```python
INSTALLED_APPS = [
    # The rest of the values are truncated
    "allauth",
    "allauth.socialaccount",
]
```
```

然后，我们要支持的每个提供商也必须添加到 `INSTALLED_APPS` 列表中——例如，请看下面的代码片段：

```
```python
INSTALLED_APPS = [
    # The rest of the values are truncated
    "allauth.socialaccount.providers.github",
    "allauth.socialaccount.providers.google",
]
```
```

完成所有这些之后，我们需要运行 migrate 管理命令来创建 django-allauth 模型：

```
python3 manage.py migrate
```

完成后，可以通过 Django 管理界面添加新的社交应用（图 15.37）：

![](img/4840a43efd335abd10b93708225692d6_694_0.png)

图 15.37 – 添加社交应用

要添加社交应用，请选择一个提供商（此列表只会显示 `INSTALLED_APPS` 列表中的提供商），输入名称（可以与提供商名称相同），并输入提供商网站上的客户端 ID（稍后我们将详细介绍）。你可能还需要一个密钥和一个键。选择应应用该密钥和键的站点。（如果你只有一个 Site 实例，那么其名称无关紧要——选择它即可。站点名称可以在 Django 管理的 Sites 部分中更新。你也可以在那里添加更多站点。）

现在，我们来看看三个示例提供商所使用的令牌。

## GitHub 认证设置

可以在你的 GitHub 个人资料下设置新的 GitHub 应用。在开发期间，应用的回调 URL 应设置为 http://127.0.0.1:8000/accounts/github/login/callback/，并在部署到生产环境时更新为真实主机名。创建应用后，它将提供客户端 ID 和客户端密钥。它们分别对应 django-allauth 中的客户端 ID 和密钥。

## Google 认证设置

Google 应用的创建通过你的 Google Developers 控制台完成。在开发期间，授权重定向 URI 应设置为 http://127.0.0.1:8000/accounts/google/login/callback/，并在生产部署后更新。应用的客户端 ID 在 django-allauth 中对应 Client id，应用的客户端密钥则对应 Secret key。

## 使用 django-allauth 发起认证

要通过第三方提供商发起认证，你首先需要在 URL 映射中添加 django-allauth 的 URL。在某个 urls.py 文件的 urlpatterns 中添加 allauth.urls：

```
urlpatterns = [path('allauth', include('allauth.urls')),]
```

然后，你将能够使用诸如 http://127.0.0.1:8000/allauth/github/login/?process=login、http://127.0.0.1:8000/allauth/google/login/?process=login 之类的 URL 发起登录。django-allauth 将为你处理所有重定向，并在用户返回站点时创建/认证 Django 用户。你可以在登录页面上放置带有“使用 GitHub 登录”或“使用 Google 登录”等文字的按钮，这些按钮链接到上述 URL。

## django-allauth 的其他功能

除了使用第三方提供商进行认证之外，django-allauth 还可以添加一些 Django 没有内置的有用功能。例如，你可以配置它要求用户提供电子邮件地址，并让用户通过点击收到的确认链接来验证其电子邮件地址，然后才能登录。django-allauth 还可以处理生成用于密码重置的 URL，并通过电子邮件发送给用户。你可以在 https://django-allauth.readthedocs.io/en/stable/overview.html 找到解释这些功能及更多内容的 django-allauth 文档。

现在我们已经深入介绍了前四个第三方应用，并简要概述了 django-allauth，你可以开始本章的练习了。在这个练习中，我们将重构所使用的 ModelForm 实例，以便使用 CrispyFormHelper 类。





## Activity 15.01 – 使用 FormHelper 更新表单

在本活动中，我们将更新 ModelForm 实例（PublisherForm、ReviewForm 和 BookMediaForm）以使用 crispy FormHelper 类。使用 FormHelper，我们可以在 Form 类本身内部定义提交按钮的文本。然后我们可以将 `<form>` 渲染逻辑从 instance-form.html 模板中移出，并用 crispy 模板标签替换它。

以下步骤将帮助你完成活动：

- 1. 创建一个 InstanceForm 类，该类继承自 forms.ModelForm。这将成为现有 ModelForm 类的基础。
- 2. 在 InstanceForm 的 `__init__` 方法中，在 self 上设置一个 FormHelper 实例。
- 3. 向 FormHelper 添加一个 Submit 按钮。如果表单是用 instance 实例化的，则按钮文本应为 Save；否则，应为 Create。
- 4. 将 PublisherForm、ReviewForm 和 BookMediaForm 更新为从 InstanceForm 继承。
- 5. 更新 instance-form.html 模板，使 `<form>` 使用 crispy 模板标签进行渲染。其余的 `<form>` 可以删除。
- 6. 在 book_media 视图中，is_file_upload 上下文项不再需要。

完成后，你应该会看到表单以 Bootstrap 主题呈现。图 15.38 显示了新建出版商页面：

![](img/4840a43efd335abd10b93708225692d6_696_0.png)

图 15.38 – 新建出版商页面

图 15.39 显示了新建评论页面：

图 15.39 – 新建评论表单

最后，图书媒体页面显示在图 15.40 中：

![](img/4840a43efd335abd10b93708225692d6_698_0.png)

图 15.40 – 图书媒体页面

请注意，表单仍然可以正常工作并允许文件上传。django-crispy-forms 已自动向 `<form>` 添加了 `enctype="multipart/form-data"` 属性。你可以通过查看页面源代码来验证这一点。

### Note

本活动的解决方案可在 https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions 上找到。

## Summary

在本章中，我们介绍了五个可以增强网站的第三方 Django 应用。我们安装并设置了 `django-configurations`，它使我们能够轻松地在不同的设置之间切换并使用环境变量更改它们。`dj-database-url` 也帮助了设置，使我们能够使用 URL 进行数据库设置更改。我们看到了 Django Debug Toolbar 如何帮助我们了解应用程序正在做什么并帮助我们调试问题。`django-crispy-forms` 不仅可以使用 Bootstrap CSS 渲染我们的表单，还允许我们通过将行为定义为表单类本身的一部分来节省代码。我们简要查看了 `django-allauth`，并看到了它如何集成到第三方身份验证提供程序中。在本章的活动中，我们更新了 `ModelForm` 实例以使用 `django-crispy-forms FormHelper`，并通过使用 `crispy` 模板标签从模板中移除了一些逻辑。

在下一章中，我们将研究如何将 React JavaScript 框架集成到 Django 应用程序中。
