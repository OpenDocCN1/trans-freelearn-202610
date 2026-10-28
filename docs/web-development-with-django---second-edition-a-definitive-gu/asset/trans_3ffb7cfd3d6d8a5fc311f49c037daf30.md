

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

