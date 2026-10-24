

在 *Figure 2.6* 中，我们导入了 `sys` 库（也称为 system），并且 `sys` 有一个名为 `version` 的属性，用于显示当前的 Python 版本以及一些系统信息。有关 sys 和 IDLE 的更多信息，请分别访问 `https://docs.python.org/3/library/sys.html` 和 `https://docs.python.org/3/library/idle.html`。

默认的 Python 安装会安装大部分内部或默认库（sys、urllib、os、math 等），但如果我们想要使用不可用的库，系统会抛出 `ModuleNotFoundError` 错误，如 *Figure 2.7* 所示：

```
```text
>>> import urllib
>>> import requests
Traceback (most recent call last):
  File "<pyshell#3>", line 1, in <module>
    import requests
ModuleNotFoundError: No module named 'requests'
>>> import pycurl
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    import pycurl
ModuleNotFoundError: No module named 'pycurl'
```
```

Figure 2.7: ModuleNotFoundError encountered when importing the requests library

与 *Figure 2.7* 类似的情况需要额外安装此类库，可以使用 pip 来处理。

到目前为止，Python 已安装完成，并且我们已使用 CLI 和 Python 的 **Graphical User Interface (GUI)**、IDLE 验证了安装。为了设置额外的库，例如 requests，我们首先需要采取一些额外的步骤，我们将在下一节中介绍。

## Creating a virtual environment

在继续安装所需库或 Python 默认不可用的额外库之前，建议为项目创建一个虚拟环境。

虚拟环境为项目及其所需库维护一个目录，以避免库与系统之间的冲突。它通常为具有特定需求和规范的目标项目提供完整的环境。有关虚拟环境的更多信息，请访问 `https://docs.python.org/3/tutorial/venv.html` 或 `https://docs.python-guide.org/dev/virtualenvs/`。

我们将在虚拟环境中安装本书所需的所有库和工具。因此，让我们开始创建环境，并通过以下步骤使用 pip 加载所需库：

- 1. 首先创建一个目录，例如 HOWScraping2E（这是我们希望将本书中的所有代码放置的位置），在根驱动器中（C:\HOWScraping2E>）。
- 2. 在 HOWScraping2E 中，运行以下命令：
- 3. 要激活我们创建的环境（secondEd），请使用 secondEd\Scripts 运行 activate 命令，如 Figure 2.8 所示：
- 4. 要停用或关闭环境，我们只需发出 deactivate 命令，例如，(secondEd) C:\HOWScraping2E> deactivate。建议在结束时或关闭系统前停用环境。

```
C:\HOWScraping2E>python -m venv secondEd
```

这里我们使用 venv 创建了一个名为 secondEd 的虚拟环境。

此步骤还在 HOWScraping2E 目录内创建了一个名为 secondEd 的目录，其中包含几个文件夹。

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_52_0.png)

Figure 2.8: Activating the virtual environment

虚拟环境已成功激活，因为 secondEd 环境可见于命令提示符的开头，如 Figure 2.8 所示。

现在位置和环境已准备就绪，我们需要使用 pip 安装所需的库和工具。pip 包管理系统用于安装和管理用 Python 编写的软件包。有关更多信息，请访问 https://pypi.org/project/pip/ 和 https://packaging.python.org/en/latest/tutorials/installing-packages/。

让我们获取我们将要使用的 pip 的版本信息，以及 pip 在我们虚拟环境中的路径，如 Figure 2.9 所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_52_1.png)

Figure 2.9: pip location and version, and Python version

> **Important note**
> 
> 通常，`lib` 文件夹包含系统默认 Python 安装的库，而外部库位于 `site-packages` 内。如 Figure 2.9 所示，我们现在在虚拟环境中以及项目特定或专用文件夹内拥有 `lib` 和 `site-packages`。因此，任何库的安装或更新都将在我们的目标环境中进行。
> 
> 如果你更喜欢使用 *Anaconda*，请参考 `https://docs.anaconda.com/free/navigator/tutorials/manage-environments/` 和 `https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html`。

到目前为止，我们的环境已准备就绪并正常工作。我们已验证 pip 存在于我们的系统中。在下一节中，我们将安装所需的库。

## Installing libraries

在本节中，我们将在环境中安装一些库，例如 `requests`、`jupyterlab` 以及根据需要安装的其他库。

Python 笔记本现在需求量很大——它们提供代码的清晰可见性，并对输入和输出有很大控制。此外，导出功能非常有用，可用于导出代码以进行演示或解释。让我们安装 JupyterLab（`https://jupyter.org`），以便我们可以使用 Python 笔记本（`.ipynb` 文件）。我们可以使用 pip install 按以下命令安装 JupyterLab：

```
```console
(secondEd) C:\HOWScraping2E> pip install jupyterlab
```
```

成功安装 JupyterLab 后，让我们运行它并检查其他库的可用性（在 JupyterLab 设置期间会安装许多依赖库）。要加载 `jupyterlab`，请使用以下命令：

```
```console
(secondEd) C:\HOWScraping2E>jupyter-lab
```
```

这将在默认浏览器中打开一个新窗口，指向 `http://localhost:8888/lab`，如 *Figure 2.10:* 所示：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_54_0.png)

Figure 2.10: JupyterLab (the default window in Google Chrome)

> **Important note**
> 
> 有关使用 Python 笔记本和 JupyterLab 的更多信息，请访问 `https://docs.jupyter.org/en/latest/`。此外，你可以创建一个笔记本并检查是否需要安装其他库——如果缺少，我们也可以从笔记本本身安装它们（使用 `!pip install requests`）。

我们决定安装和使用 JupyterLab 非常方便，因为主要库如 requests 会自动安装并可供我们使用。如 *Figure 2.7* 所示，requests 库在默认 Python 安装中不可用。让我们验证我们的其他所需库是否工作正常；在使用笔记本中导入它们，如 *Figure 2.11* 所示（代码可在 `https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter02/test_install_libraries.ipynb` 文件中找到）：

```
[2]: import requests
```

```
[3]: requests.__version__
```

```
[3]: '2.28.1'
```

```
[4]: import urllib
```

```
[5]: list(dir(urllib))
```

```
[5]: ['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', 'error', 'parse', 'request', 'response']
```

Figure 2.11: Verifying the requests and urllib libraries

如果你不想使用 JupyterLab，可以从终端使用以下代码设置库：

```
(secondEd) C:\HOWScraping2E> pip install requests
```

现在，我们的环境已设置完成，所需库已在我们的系统中。我们将在下一节中使用这些库与 URL 通信（向网络发送请求并收集响应）。

## Loading URLs

加载 URL 通常涉及 HTTP 通信，例如向服务器发送请求并从服务器接收响应。本章即将介绍的 URL 处理和操作以及 Implementing HTTP methods 部分将更详细地探讨这些主题。数据提取活动将在第 3 章中介绍。

> **Important note**
> 在使用 Python 加载 URL 之前，最好先验证 URL 是否正常工作并包含我们正在寻找的数据。这种基本的可行性检查将在运行和管理项目时节省大量资源。

