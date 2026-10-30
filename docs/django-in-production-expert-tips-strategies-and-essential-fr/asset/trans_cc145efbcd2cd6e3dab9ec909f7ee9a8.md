

我们都喜欢 `django-admin` 命令以及在创建新项目或应用时它提供的所有样板代码。然而，在处理大型项目时，默认的项目结构就没那么有用了。这是因为当我们在生产环境中使用 Django 时，有许多其他需要整合到项目中的组件。项目结构以及与项目一起使用的其他工具总是带有主观性；你在当前项目中认为可行的方法，可能在一年后创建的下一个项目中就不适用了。

### 重要说明

互联网上有大量资源会建议不同的项目结构。我最喜欢的一个是 `django-cookiecutter`。它为项目集成了大量工具，并提供了一个可遵循的结构，但对于任何新手来说，由于它集成了许多可能用不到的第三方工具，以及一些可能不理解的配置，开始使用起来可能会令人望而生畏。但与其担心这些，你只需跟随本书即可！

我们将创建自己的极简项目结构，并逐步将其他工具集成到项目中。首先，让我们使用 Django 创建 `hello_world` 项目：

```
mkdir hello_world && cd hello_world
mkdir backend && cd backend
django-admin startproject config .
```

这里，我们创建了项目文件夹 `hello_world`，然后在其内部创建了一个名为 `backend` 的子文件夹。我们使用 `backend` 文件夹来存放所有与 Django 相关的代码；随着我们学习更多关于 CI/CD 功能并将更多工具整合到项目中，我们将在与 `backend` 子文件夹同一层级创建更多文件夹。最后，我们使用 Django 管理命令创建了项目。

### 重要说明

注意 `startproject` 命令后附加的 `.`（点）；这告诉 Django 管理命令在当前文件夹中创建项目，而不是创建一个名为 `config` 的单独文件夹来存放项目。默认情况下，如果你不添加 `.`，那么 Django 将在其中创建名为 `config` 的额外文件夹，项目结构将在其中创建。为了更好地理解，你可以尝试带 `.` 和不带 `.` 的命令，以清楚地了解它对结构的影响。

执行这些命令后，我们应该能看到此处所示的项目结构：

![Figure 1.1: Expected project structure after executing the commands](image/B18867_01_01.jpg) 

Figure 1.1: Expected project structure after executing the commands

现在项目结构已准备就绪，让我们运行 `python manage.py runserver` 来验证我们的 Django 项目。我们应该在 shell 中看到以下输出：

![Figure 1.2: The python manage.py runserver command’s output in the shell](image/B18867_01_02.jpg) 

Figure 1.2: The python manage.py runserver command’s output in the shell

请忽略控制台中以红色显示的未应用迁移警告 `You have 18 unapplied migrations(s)`；当我们在下一章学习更多关于数据库、模型和迁移的内容时，我们将详细介绍这个问题。

现在，打开你的浏览器并访问 `http://localhost:8000` 或 `http://127.0.0.1:8000`（如果前者无法加载）。我们将看到如图 *Figure 1**.3* 所示的屏幕，这验证了我们的服务器正在成功运行：

### 请注意

你可以使用 `http://localhost:8000` 或 `http://127.0.0.1:8000` 在浏览器中打开 Django 项目。如果你在访问 `http://localhost:8000` 时遇到任何错误，请尝试对本书中提到的所有 URL 使用 `http://127.0.0.1:8000`。

![Figure 1.3: Our Django server running successfully with port 8000](image/B18867_01_03.jpg) 

Figure 1.3: Our Django server running successfully with port 8000

现在，让我们创建我们的第一个 `hello_world` 视图。为此，请按照以下步骤操作：

1. 打开 `config/urls.py` 文件。
2. 在 `hello_world` 中添加一个新的 `view` 函数。
3. 将 `hello_world` 视图函数链接到 `hello-world` 路径。

我们的 `config/urls.py` 文件应如下所示的代码片段：

```
from django.contrib import admin
from django.http import HttpResponse
from django.urls import path
def hello_world(request):
    return HttpResponse('hello world')
urlpatterns = [
    path('admin/', admin.site.urls),
    path('hello-world/', hello_world)
]
```

4. 打开 `http://127.0.0.1:8000/hello-world/` 以获取如图 *Figure 1**.4* 所示的结果：

![Figure 1.4: http://127.0.0.1:8000/hello-world/ browser response](image/B18867_01_04.jpg) 

Figure 1.4: http://127.0.0.1:8000/hello-world/ browser response

到目前为止，我们已经了解了如何创建项目文件夹结构以及如何在 Django 中创建第一个视图。我们使用的示例是最小的 Django 项目示例之一，不涉及应用。因此，让我们看看如何在 Django 中创建应用，以帮助我们更好地管理项目。

## 在 Django 中创建我们的第一个应用

Django 应用可以被视为在大型项目中执行单一功能的小型包。Django 提供了管理命令来在项目中创建新应用；这些是内置命令，用于执行重复且复杂的任务。Django 社区喜欢管理命令，因为它们减少了大量手动工作并封装了许多复杂任务，例如迁移等。我们将在后续章节中学习更多关于 Django 管理命令的内容，在那里我们将创建一个自定义管理命令。然而，每当你看到 `manage.py` 后跟一个命令时，那就是一个 Django 管理命令。

那么，让我们使用 Django 管理命令接口创建一个新的 `demo_app`：

```
python manage.py startapp demo_app
```

运行此命令将创建此处所示的文件夹结构：

![Figure 1.5: Project structure with demo_app added](image/B18867_01_05.jpg) 

Figure 1.5: Project structure with demo_app added

我们可以看到，已创建了一个 `demo_app` 文件夹，其中包含 Django 为新应用生成的样板代码。

### 重要说明

每当我们创建一个新应用时，必须做的一件重要事情是告诉 Django 关于这个新应用。不幸的是，使用 Django 管理命令创建新应用时，这不会自动发生。这是一个手动过程，你需要在 `settings.py` 文件中的 `INSTALLED_APPS` 列表中添加新应用的详细信息。Django 使用它来识别添加到项目中的所有依赖应用，并检查任何数据库相关更改，甚至注册信号和接收器。

虽然目前由于我们没有使用 `models` 让 Django 自动识别任何数据库相关更改，添加新应用到 `INSTALLED_APPS` 列表不是必需的，但仍然是良好的实践。我们的 `INSTALLED_APPS` 列表应如下所示：

```
INSTALLED_APPS = [
    ...
    'django.contrib.staticfiles',
    'demo_app',
]
```

请记住在每个新应用条目后加上逗号（`,`）；这是开发人员最常犯的错误之一，它会导致 Django 将两个应用名称拼接在一起，并在最终纠正之前生成语法错误。

### 重要说明

在 Django 中，第三方应用集成也是通过 `INSTALLED_APPS` 完成的，因此我们将在以下部分看到有关如何维护 `INSTALLED_APPS` 的最佳实践。

现在我们已经使用样板代码创建了一个新的 Django 应用，我们可以将应用视图与 `urls.py` 链接起来。

## 使用 urls.py 链接应用视图

在本节中，我们将链接由 Django 管理命令创建的 `views.py`。`views.py` 用于向应用端点添加业务逻辑。就像我们在上一节中添加了 `hello_world` 函数视图一样，我们可以在 `views.py` 文件中添加不同的函数式或基于类的视图。

