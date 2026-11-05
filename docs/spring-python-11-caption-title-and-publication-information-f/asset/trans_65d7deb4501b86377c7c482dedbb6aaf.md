
![](img/bcf886d8b665372ee8afbb620d4273bb_16_0.png)

警告或重要注意事项会以如下框形式出现。

![](img/bcf886d8b665372ee8afbb620d4273bb_16_1.png)

提示和技巧会以如下方式出现。

## 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢或可能不喜欢的部分。读者的反馈对我们开发您真正能充分利用的书目非常重要。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及书名即可。

如果您需要某本书并希望我们出版，请在 www.packtpub.com 上的 SUGGEST A TITLE 表单中留言，或发送电子邮件至 suggest@packtpub.com。

如果您在某个主题方面有专业知识，并有兴趣撰写或为相关书籍贡献内容，请参阅 www.packtpub.com/authors 上的作者指南。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项帮助以助您充分利用购买。

![](img/bcf886d8b665372ee8afbb620d4273bb_16_2.png)

**下载本书的示例代码**
访问 https://www.packtpub.com//sites/default/files/downloads/0660_Code.zip 直接下载示例代码。
可下载文件包含使用说明。

## 勘误

尽管我们已尽最大努力确保内容的准确性，但错误仍可能发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。这样做可以避免其他读者感到沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的图书，点击 **告诉我们** 链接，并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受并添加到现有勘误列表中。任何现有勘误都可以通过从 http://www.packtpub.com/support 选择您的书名来查看。

## 盗版

互联网上版权材料的盗版是各媒体持续存在的问题。在 Packt，我们非常重视对版权和许可的保护。如果您在互联网上发现我们作品的任何非法复制品，无论形式如何，请立即提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上疑似盗版材料的链接。

我们感谢您帮助保护我们的作者，以及我们为您带来有价值内容的能力。

## 问题

如果您在本书的任何方面遇到问题，可以联系 questions@packtpub.com，我们将尽力解决。

## 1 使用 Spring Python 入门

Spring Python 采用了 Spring Framework 和 Spring Security 的概念，并将其带入 Python 世界。它并不是代码逐行移植。相反，它汲取了在 Java 领域发现的一些强大思想，并在 Python 世界中务实地应用它们。

Spring（Java）提供许多简单易用的功能部件来组装应用程序，而不是一个用于扩展的单体框架。Spring Python 使用相同的方法。这意味着我们可以根据每个 Python 应用程序的需求使用尽可能少或多的 Spring Python 来完成工作。

在本章中，我们将学习：

- 了解 Spring Python 的非侵入式 API，它使使用其他库变得容易，而无需对自己的代码库进行重大更改
- Spring Python 如何使用控制反转来解耦对象的创建与使用，从而赋能开发者
- Spring Python 如何通过提供非侵入式 API 来帮助专业 Python 开发者轻松访问高级服务
- Spring Python 为专业 Java 开发者提供通过 Python/Jython/Java 组合轻松混合 Python 和 Java 的方式
- 如何从二进制和源代码安装库
- Spring Python 的可扩展性如何，以及一些指向 Spring Python 社区的链接

### 面向 Python 开发者的 Spring Python

您已经选择了最受欢迎且技术上功能强大的动态语言之一 Python 来开发软件。Spring Python 使解决 Python 开发者每天遇到的常见问题变得更加容易。

### 探索 Spring Python 的非侵入式特性

Spring Python 具有非侵入式特性，这意味着可以轻松采用满足需求的部分，而无需重写大量代码。例如，Pyro (http://pyro.sourceforge.net) 是一个第三方库，可轻松进行远程过程调用。

为了演示 Spring 的非侵入式方法，让我们编写一个简单的服务，使用 Pyro 的 API 将其发布为 Web 服务，然后使用 Spring Python 对 Pyro 的封装将其发布。这将展示 Spring Python 如何简化我们的 API 访问，以及它如何使第三方库更容易使用，而无需对自己的代码进行太多重构。

- 1. 首先，让我们编写一个简单的服务，从 Web 请求字符串中解析参数：
```python
class ParamParser(object):
    def parse_web_parms(self, parm):
        return [tuple(p.split("=")) for p in parm.split("&")]
```
- 2. 现在我们可以编写一段简单、功能性的代码，使用我们的服务以获得可工作的版本。
```python
parser = ParamParser()
parser.parse_web_parms("pages=5&article=Spring_Python")
```
这只是实例化 `ParamParser` 并访问函数。要使其成为有用的互联网服务，它需要在中央服务器上实例化，并应配置为监听来自客户端的调用。
- 3. 下一步是使用 Pyro 的 API 将其作为 Web 服务发布。这将使其可被多个 Pyro 客户端访问。为此，我们定义一个守护进程，在端口 9000 上托管我们的服务并对其进行初始化。
```python
import Pyro

daemon = Pyro.core.Daemon(host="localhost", port="9000")
Pyro.core.initServer()
```
- 4. 接下来，我们创建一个 `Pyro` 对象实例作为我们服务的代理，以及我们 `ParamParser` 的一个实例。我们配置代理将所有方法调用委托给我们的服务。
```
pyro_proxy = Pyro.core.ObjBase()
parser = ParamParser()
pyro_proxy.delegateTo(parser)
```
- 5. 最后，我们将 `pyro_proxy` 对象注册到 `daemon`，并启动监听-分发循环，使其准备好处理请求：
```
daemon.connect(pyro_proxy, "mywebservice")
daemon.requestLoop(True)
```
当我们运行此服务器代码时，我们的 `ParamParser` 的一个实例将被创建并在 `PYROLOC://localhost:9000/mywebservice` 上发布。

为了使此服务完整，我们需要创建一个 `Pyro` 客户端来调用我们的服务。代理使用 `Pyro` 库无缝地在有线上传输 Python 对象，在本例中为请求参数的元组。
```
import Pyro

url_base = "PYROLOC://localhost:9000"
client_proxy = Pyro.core.getProxyForURI( \
    url_base + "/mywebservice")
print client_proxy.parse_web_parms( \
    "pages=5&article=Spring_Python")
```
`Pyro` 库易于使用。一个关键因素是我们的 `ParamParser` 从未与用于将其提供给远程客户端的 `Pyro` 机制紧密耦合。然而，它非常侵入性。

如果我们已经在单台机器上开发了一个简单的应用程序，其中大量方法使用我们的实用程序？为了将我们的应用程序转换为客户端-服务器应用程序，我们必须在每次调用时重写它以使用 `Pyro` 客户端代理模式。如果我们遗漏任何实例，我们将会有需要清理的错误。如果我们编写了自动化测试，它们也必须重写。将简单的单机应用程序转换为多节点应用程序会迅速产生大量工作。

这就是 Spring Python 发挥作用的地方。它提供了一种不同的创建对象的方式，使我们能够轻松地用远程机制（如 `Pyro`）替换本地对象。稍后，我们将更详细地探讨 Spring Python 容器的概念。
