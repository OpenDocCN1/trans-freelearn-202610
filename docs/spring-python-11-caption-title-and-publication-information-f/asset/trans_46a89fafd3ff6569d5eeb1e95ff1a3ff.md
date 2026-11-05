
## 从预构建的二进制下载安装

二进制文件由 SpringSource 以 tar 包的形式托管在社区部分。它们是非操作系统特定的，应该可以在任何平台上运行。

1. 访问 http://www.springsource.org/download/community。你可以填写注册信息，或直接点击链接前往下载站点

![](img/bcf886d8b665372ee8afbb620d4273bb_32_0.png)

2. 点击 Spring Python 查看最新版本：

![](img/bcf886d8b665372ee8afbb620d4273bb_33_0.png)

3. 下载 `springpython-<release>.tar.gz` 以获取核心库

4. 解压 tar 包，并进入包含 `setup.py` 的目录

5. 输入以下命令安装 Spring Python

> 注意：此步骤可能需要管理员权限！
> $ python setup.py install

6. 现在你应该能够测试安装。

```
$ python
>>> import springpython
>>> dir()
['__builtins__', '__doc__', '__name__', 'springpython']
```

## 从源代码安装

Spring Python 可以像许多其他开源项目一样从源代码安装。代码托管在 Subversion 仓库中。以下步骤将帮助你下载代码并在机器上安装：

1. 输入以下命令检出最新的 trunk 仓库：
```bash
$ svn checkout https://src.springframework.org/svn/se-springpython-py/trunk/springpython springpython
```
这将在本地创建包含最新更改的目录 `springpython`。

2. 进入包含 `build.py` 的目录。该脚本是构建、打包和测试 Spring Python 的关键工具。输入以下命令生成可安装的 Spring Python 包：
```bash
$ ./build.py -package
```

3. 进入 `target/artifacts`，该目录包含新生成的 tar 包。

4. 解压 tar 包 `springpython-<release>.tar.gz`，并进入包含 `setup.py` 的目录，然后输入以下命令安装 Spring Python：
```bash
$ python setup.py install
```

5. 现在你应该能够测试安装。
```bash
$ python
>>> import springpython
>>> dir()
['__builtins__', '__doc__', '__name__', 'springpython']
```

6. 使用 Subversion 仓库获取源代码可让你轻松安装新更新。

7. 输入以下命令更新你的检出。
```bash
$ svn update /path/for/springpython
```

8. 使用 `build.py`、`target/artifacts` 和 `setup.py` 重复上述步骤。

> 注意：此步骤可能需要管理员权限。
> $ python setup.py install

> 正式发布版本位于 Subversion 仓库的 tags 部分，紧邻 trunk。

> 你也可以使用 SpringSource 的 Fisheye 查看器在 https://fisheye.springframework.org/browse/se-springpython-py 可视化检查代码。这提供了一种查看代码、变更集和变更日志的方式。

## Spring Python 社区

Spring Python 重视其用户社区，并在关键功能上以用户请求为依据。Spring Python 中目前的许多功能都是基于用户请求、提交的补丁和反馈添加的。

- `http://springpython.webfactional.com`—项目网站，包括参考文档的链接

![](img/bcf886d8b665372ee8afbb620d4273bb_35_0.png)

- `http://forum.springsource.org/forumdisplay.php?f=45`—社区讨论论坛
- `http://blog.springpython.webfactional.com`—Spring Python 作者的博客网站
- http://lists.springsource.com/archives/springpython-users — Spring Python 用户的邮件列表
- http://www.linkedin.com/groups?gid=1525237 — Spring Python 用户的 LinkedIn 群组

## 摘要

我们对 Spring Python 的各个部分进行了快速浏览。示例展示了 Spring Python 如何让 Python 和 Java 开发者都更轻松。

在本章中，我们了解到：

- Spring Python 具有非侵入式 API，使你无需对自己的代码库进行重大更改即可轻松使用其他库
- Spring Python 具有一个容器，将对象创建与对象使用解耦，从而赋能开发者
- Spring Python 通过提供非侵入式 API 来帮助专业 Python 开发者轻松访问高级服务
- Spring Python 为专业 Java 开发者提供了一种简单的方式，通过 Python/Jython/Java 的组合将 Python 和 Java 混合使用
- Spring Python 可以从二进制和源代码安装
- Spring Python 非常可扩展，并且很容易让我们添加新组件
- 有一个用户社区，我们可以在其中发布问题、答案、想法和补丁

在下一章中，我们将深入 Spring Python 的核心，即其 `Inversion of Control` 容器。

## 2

## Spring Python 的核心——控制反转

许多软件团队寻求在不牺牲质量的情况下提高生产力的方法。这导致自动化测试增加，进而引发了使应用程序更易于测试的兴趣。许多自动化测试框架通过用测试替身交换协作对象来提供罐装响应，从而促进对象隔离。这要求开发者以尽可能少的更改插入测试替身。

有时，管理者通过削减开发和集成硬件来压缩预算。这使开发者面临在比生产环境更小的环境中开发的困境。软件开发者需要能够在不同的测试配置和部署场景之间切换的能力。

销售给客户的软件产品通常需要在现场进行灵活调整，无论是通过公司代表还是客户本人。

在本章中，我们将探讨 Spring Python 的 **控制反转 (IoC)** 容器如何通过将关键对象的创建移入容器定义来满足所有这些需求，从而在不影响核心业务代码的情况下实现灵活调整。

本章将涵盖：

- IoC 容器如何使隔离测试对象变得容易，以便进行自动化测试。
- 对 IoC 的详细解释，附带高层图表。我们将学习依赖注入是如何一种 IoC 类型，并使用好莱坞原则 *别叫我们，我们会叫你！*
- Spring Python 的 IoC 容器，处理依赖解析、设置属性和延迟初始化。它还为开发者按需使用提供扩展点。
- Spring Python 对动态语言中 IoC 的社区争论的回应。
- 如何轻松地将 Python 和 Java 组件混合使用，可以有多种方式，并为开发者提供选择最适合其需求的好方法。

### 用测试替身交换生产代码

作为开发者，我们需要能够用模拟对象和存根交换生产对象的能力。

![](img/bcf886d8b665372ee8afbb620d4273bb_38_0.png)

> 模拟对象和存根是生成用于测试代码的罐装响应的两种方式。**模拟对象** 用于生成罐装方法调用。**存根** 用于生成罐装数据。这两者都是模拟代码必须工作的外部组件的有用工具。更多详情请参阅 http://martinfowler.com/articles/mocksArentStubs.html

在此示例中，我们将探讨 IoC 容器如何使交换生产对象与模拟实例变得容易。我们从为 wiki 引擎开发一个简单服务以及一些自动化测试开始。

1. 首先，让我们为 wiki 引擎定义一个简单服务。下面的 `WikiService` 有一个函数，它查询 MySQL 数据库并返回页面的点击次数以及每次编辑的读取比率。

```
class WikiService(object):

    def __init__(self):
        self.data_access = MySqlDataAccess()
```
