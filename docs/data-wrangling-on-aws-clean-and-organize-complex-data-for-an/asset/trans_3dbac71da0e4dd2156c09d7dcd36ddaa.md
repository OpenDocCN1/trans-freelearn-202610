

## 使用 Lambda 函数的标准与自定义安装

对于大多数用例，例如项目 A 的 IT 人员处理小体量数据的情况，我们希望将数据集成代码部署到无服务器（serverless）环境中并执行。AWS Lambda 是一种无服务器的事件驱动计算服务，让你能够在几乎任何类型的应用程序或后端服务上运行代码，而无需配置或管理服务器。Lambda 预装了各种重要库，但当部署到 Lambda 函数时，用户必须将与代码一起打包那些默认环境中不可用的库。

对于较小的库，用户可以将依赖项与代码一起打包，并将其上传到 Lambda 函数。我们将学习如何创建包含 Lambda 函数依赖库的 ZIP 归档文件 ([创建包含依赖项的软件包](https://docs.aws.amazon.com/Lambda/latest/dg/python-package.html#python-package-create-package-with-dependency))。为此，请创建一个新的 Python 虚拟环境，并在虚拟环境中安装新软件包。这将在你的机器上的 pip 安装路径下安装依赖软件包。然后，你可以将这些软件包移动到 Lambda 函数的部署文件夹中，并将其归档为 ZIP 文件，以便上传到 Lambda 函数。

Lambda 函数还可以拥有层（layers），它们保存为 ZIP 文件，包含额外的代码/库。它也可以包含自定义运行时、数据或配置文件。在 Lambda 函数中使用层可以减小部署包的大小，并且同一个 Lambda 层可以在多个 Lambda 函数之间共享，从而促进可重用代码组件。此外，这有助于在 different 团队之间分离管理代码片段的职责。另外，当你在单个 ZIP 文件中部署代码和依赖项时，Lambda 函数在控制台中是不可编辑的，这增加了代码维护的难度。目前，可以使用 `.zip` 文件归档创建 Lambda 层。

基于上述所有内容，我们在哪里使用 Lambda 层？考虑这样一种情况：存在一个中央数据仓库，不同的项目团队使用 Lambda 函数作为微服务从中访问数据，并将其用于不同目的。需求是只提供组织内特定用户组访问权限，同时在访问数据时执行数据脱敏。数据库管理员还担心过多的并发用户访问数据库以及留下空闲打开的连接可能会使数据库不堪重负。各个项目团队也担心使用和 maintenance 数据库驱动程序以连接到仓库，并且他们没有管理经验数据库连接状态、配置等的经验。理想的解决方案是创建一个符合上述所有要求的用于访问仓库的 Lambda 层，该层将由数据库管理团队维护，而各个团队可以利用该 Lambda 层在 Lambda 函数中开发其代码。这促进了代码重用、职责分离以及更快的开发周期。

在相同的上下文中，用户可以从 Lambda 层使用 AWS SDK for pandas。在 Lambda 代码中使用 AWS SDK for pandas 有两个选项。

### 标准安装 – Lambda 管理的层

在 AWS 控制台中创建一个使用 Python 3.9 运行时的 Python Lambda 函数。向下滚动 Lambda 函数概览屏幕，直到页面底部，在那里你可以找到 **Layers**（层）部分：

![](img/e28602b44ec813a47df57c96b204412c_103_0.png)

然后你可以选择添加一个层；选择 **AWSSDKPandas-Python39** 层，如以下屏幕截图所示。随着 AWS 团队添加新发行版，版本号将来可能会改变。这是 AWS 团队为你精心策划的一个层，它移除并优化了组件，从而减小了软件包的大小。

### 自定义安装

- **II. 使用链接将整个 git 仓库克隆到本地机器/EC2 以进行进一步开发：**

```bash
(awspandas) $ git --version
git version 2.37.3
(awspandas) $ git clone https://github.com/aws/aws-sdk-pandas
Cloning into 'aws-sdk-pandas'...
remote: Enumerating objects: 13000, done.
remote: Counting objects: 100% (565/565), done.
remote: Compressing objects: 100% (264/264), done.
remote: Total 13000 (delta 388), reused 431 (delta 299), pack-reused 12435
Receiving objects: 100% (13000/13000), 8.00 MiB | 7.91 MiB/s, done.
Resolving deltas: 100% (9773/9773), done.
(awspandas) $
```

图 3.5：awswrangler git 副本

- **2. 进入代码库并探索代码。** 你可以修改现有代码并添加额外模块以满足项目需求：
  - **I. 导航到 awswrangler 的安装路径并探索不同的文件夹。** 你应该在名为 `awswrangler` 的文件夹下看到代码，并且为不同服务有单独的文件夹。
  - **II. 为了学习目的，让我们修改 `__init__.py` 文件**，该文件将在我们在 Python 代码中导入 `awswrangler` 时执行。探索为各种 AWS 服务/awswrangler 功能导入的模块。你可以看到一条导入语句，它从 `__metadata__.py` 文件中获取变量。按照此处所示，向导入语句附加一个新变量 `__message__`，并保存文件。

```python
from awswrangler.__metadata__ import __description__, __license__, __title__, __version__, __message__
# noqa
from awswrangler.__config import config # noqa
```

图 3.6：awswrangler init.py 文件

  - **III. 打开 `__metadata__.py` 文件并修改 `__description__` 字段**，如下所示。另外，添加另一个变量 `__message__`，它在上一步骤中被导入到 `__init__.py` 模块中，然后保存文件。

```python
__title__: str = "awswrangler"
__description__: str = "Pandas on AWS. Updated for custom installation."
__version__: str = "2.17.0"
__license__: str = "Apache License 2.0"
__message__: str = "This was changed as a part of Learning exercise."
```

图 3.7：awswrangler metadata.py 文件

- **3. 从你在本地机器或 EC2 实例上修改的自定义代码安装 `awswrangler`：**
  - **I. 导航到顶层文件夹，那里有 `awswrangler` 代码。**
  - **II. 执行 `pip3 install .` 命令**，如下所示。这将在你的机器上安装 `awswrangler`，使用包含你自定义更改的本地代码库。

```bash
(awspanandas)$ cd aws-sdk-pandas
(awspanandas)$ ls
CODE_OF_CONDUCT.md             awswrangler              test_infra
CONTRIBUTING.md                building                 tests
CONTRIBUTING_COMMON_ERRORS.md  docs                       tox.ini
LICENSE.txt                    fix.sh                   tutorials
NOTICE.txt                     poetry.lock              validate.sh
README.md                      pyproject.toml
THIRD_PARTY.txt                test.sh

(awspanandas)$ pip3 install .
Processing /Users/ssmun/Documents/Initiatives/DataWranglerBook/code/aws-sdk-pandas
Installing build dependencies ... done
Getting requirements to build wheel ... done
Preparing metadata (pyproject.toml) ... done
Collecting boto-core<2.0.0,>=1.27.11
  Downloading botocore-1.27.82-py3-none-any.whl (9.2 MB)
                                         9.2/9.2 MB 25.5 MB/s eta 0:00:00
```

图 3.8：在本地机器上安装 awswrangler

- **4. 验证并开始使用 `awswrangler` 自定义安装：**
  - **I. 打开 Python shell 并导入 `awswrangler`。**
  - **II. 打印 `__message__` 和 `__description__` 变量**，以查看我们是否从带有早期步骤中进行的更改的本地机器/EC2 实例安装了 `awswrangler` 软件包。

```python
(awspanandas)$ Python3
Python 3.9.12 (main, Apr  5 2022, 01:53:17)
[Clang 12.0.0 ]:: Anaconda, Inc. on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import awswrangler as wr
>>> print(wr.__description__)
Pandas on AWS. Updated for custom installation.
>>> print(wr.__message__)
This was changed as a part of Learning exercise.
>>>
```

图 3.9：验证带有自定义更改的 awswrangler 本地安装

- **5. 作为最后一步，你可以通过退出虚拟环境并使用 `pip3 install awswrangler` 命令安装 `awswrangler` 软件包来验证我们的更改仅限于我们的虚拟环境**（这将安装不含我们自定义代码更改的软件包，来自 PyPI 远程仓库）：

```bash
(aws pandas) $deactivate
$pip3 install awswrangler
Requirement already satisfied: awswrangler in /Users/ssmsun/miniconda3/lib/python 3.9/site-packages (2.16.1)

$python3
Python 3.9.12 (main, Apr 6 2022, 01:53:17)
[Clang 12.0.0 1:: Anconda, Inc. on darwin]
Type "help", "copyright", "credits" or "license" for more information.
>>> import awswrangler as wr
>>> print(wr.__description__)
Pandas on AWS.
>>> print(wr.__message__)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'awswrangler' has no attribute '__message__'
>>>
```

![](img/e28602b44ec813a47df57c96b204412c_102_0.png)

我们看到了如何通过简单地使用 pip 命令在本地机器/EC2 实例上安装 awswrangler 软件包。我们还从 awswrangler git 仓库下载了代码，进行了项目所需的更改，并安装了修改后的 awswrangler 版本。我们还学习了如何在虚拟环境中安装软件包，以便为一个项目所做的更改不会与其他项目冲突。

恭喜完成此步骤！你可以花一些时间在此处与该软件包互动以更深入地了解它。一旦你完成探索，让我们进入下一节，在那里我们需要帮助 IT 人员使用 Lambda 函数安装 awswrangler。

