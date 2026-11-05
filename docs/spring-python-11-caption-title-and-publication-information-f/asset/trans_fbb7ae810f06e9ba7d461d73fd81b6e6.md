
```
class TvShowContainer(PythonConfig):
    def __init__(self):
        super(TvShowContainer, self).__init__()

    @Object(lazy_init=True)
    def web_server(self):
        return PyroServiceExporter(service=TvShowService(),
                                    service_name="tvshows",
                                    service_port=9000)

    @Object(lazy_init=True)
    def web_client(self):
        myService = PyroProxyFactory()
        myService.service_url="PYROLOC://localhost:9000/tvshows"
        return myService

if __name__ == "__main__":
    container = ApplicationContext(TvShowContainer())
    container.get_object("web_server")
```

通过查询数据库中的电视节目并通过 Pyro 提供服务，这段代码演示了在不将它们混合在一起的情况下使用这些强大模块是多么容易。当事物保持简单和分离时，随着时间的推移维护软件会容易得多。

我们刚刚快速浏览了 SQL 和 Pyro 并检查了它们的低级 API。许多低级 API 需要按特定顺序执行步骤才能正确使用它们。我们刚刚看了一个 SQL 查询。模板的需求也存在于数据库事务和 LDAP 调用中。通过在 Spring Python 模板中捕获 API 的流程并允许你插入自定义代码，你可以摆脱编写管道代码，转而专注于应用程序的业务逻辑。

## 面向 Java 开发者的 Spring Python

Java 开发者经常寻求提高生产力的方法。随着 Java 虚拟机 (JVM) 上替代语言的惊人增长，开发者探索 Jython (http://www.jython.org) 作为编写脚本、片段和子系统的替代方式也就不足为奇了。虽然使用你已知的 Java 库很方便，但有时与库的 Java 习惯用法交互会很粗糙。

Spring Python 用纯 Python 编写，这使得它在 Jython 中也很容易使用。你可能会发现使用 Spring Python 的 Pythonic API 比 Spring Framework 的 Java 习惯用法更容易、更有吸引力。最终选择权在你，而且这不是非此即彼的决定；你可以在 Java 和 Python 段中同时使用这两个库。

下图展示了 Python、Jython 和纯 Java 的组合作为一个栈。

![](img/bcf886d8b665372ee8afbb620d4273bb_28_0.png)

使用 Spring Python，很容易将 Java 和 Python 混合在一起。让我们编写一个简单的示例，在其中编写一个与上一节所示的 TV 服务通信的 Jython 客户端。通过重用该代码，我们可以轻松编写客户端。为了真正展示 Python/Jython/Java 集成的威力，让我们将我们的 Jython 客户端暴露给纯 Java 消费者。

- 1. 首先，我们需要一个纯 Java 接口，让我们的 Jython 代码可以扩展它。
- 2. 接下来，我们编写 Jython 类来扩展此接口。

```
import org.python.core.PyList;

public interface JavaTvShowService {
    public PyList getTvShows();
}
```

```
class JythonTvShowService(JavaTvShowService):
    def __init__(self):
        self.container = ApplicationContext(TvShowContainer())
        self.client = container.get_object("web_client")

    def getTvShows(self):
        return self.client.get_tv_shows()
```

- 3. 为了让 Java 客户端调用 Jython，我们需要创建一个工厂来创建嵌入式 Jython 解释器。

```java
import org.python.core.PyObject;
import org.python.util.PythonInterpreter;

public class TvShowServiceFactory {
    private PyObject clazz;

    public TvShowServiceFactory() {
        PythonInterpreter interp = new PythonInterpreter();
        interp.exec("import JythonTvShowService");
        clazz = interp.get("JythonTvShowService");
    }

    public JavaTvShowService createTvShowService() {
        PyObject tvShowService = clazz.__call__();
        return (JavaTvShowService)
        tvShowService.__tojava__(JavaTvShowService.class);
    }
}
```

- 4. 最后，我们可以编写一个 main 函数，先调用工厂，然后调用 JavaTvShowService。

```java
public class Main {
    public static void main(String[] args) {
        TvShowServiceFactory factory =
            new TvShowServiceFactory();
        JavaTvShowService service =
            factory.createTvShowService();
        PyList tvShows = service.getTvShows();
        for (int i = 0; i < tvShows.__len__(); i++) {
            PyObject obj = tvShows.__getitem__(i);
            PyTuple tvShow =
                (PyTuple)obj.__tojava__(PyTuple.class);
            System.out.println("Tv show title=" + tvShow.get(0) +
                " airDate=" + tvShow.get(1) +
                " episode=" + tvShow.get(2) +
                " writer=" + tvShow.get(3));
        }
    }
}
```

我们定义了一个接口，为同时针对 Java 消费者和 Jython 服务进行编码提供了一个良好的边界。我们的 Jython 类扩展了该接口，并在创建时创建了一个 IoC 容器，该容器请求一个 `web_client` 对象。这就是我们的 `PyroProxyFactory`，它使用 Pyro 通过线路调用我们的纯 Python 服务。通过实现接口，我们方便地将远程过程调用暴露给 Java 消费者。我们的 Java 应用程序需要一个 `factory` 类来创建 Jython 解释器，进而创建我们 Jython 类的实例。我们在应用程序 main 中使用它来获取 TV 服务的实例。使用该服务，我们调用暴露的方法，这会进行 Pyro 远程调用。我们遍历接收到的列表并提取元组以打印结果。

在瞥见如何将纯 Python 服务暴露给纯 Java 消费者之后，从这里开始的选项是无穷无尽的。我们可以使用 Spring Java 的 `RmiServiceExporter` 或 `HessianServiceExporter` 将我们的 Python 服务暴露给其他协议。该服务可以集成到 Spring Integration 工作流中或成为 Spring Batch 作业的一部分。真的没有限制。我们将在 *第 10 章* 中通过案例研究更详细地探讨这一点。

我们已经了解了将 Python 服务暴露给 Java 消费者是多么容易。如果你已经熟悉 Spring Java，希望在阅读本书时，你会注意到 *Spring 方式* 不仅适用于 Java，也适用于 Python。Spring Python 使用与 Spring Java 相同的概念，与 Spring Java 共享一些类名，甚至有一个 XML 解析器可以读取 Spring Java 的格式。所有这些都增加了在对配置文件进行少量更改的同时将某些部分重写为 Python 的能力。所有这些方面都使我们更容易逐步将应用程序迁移到我们为提高生产力所需的最佳位置。

## 扩展 Spring Python

Spring Python 通过其整洁分段的模块集提供对服务、库和 API 的便捷访问。Spring Python 的灵活性源于其模块化特性。提供给你一组易于组合的小块，比提供一组单体类以供扩展更具可重用性。这些技术为轻松集成当前技术和未来技术打开了大门。

在本章中，我们已经查看了各种功能，包括 remoting、`DatabaseTemplate` 和 Python 与 Java 的集成。随着我们探索 Spring Python 的其他功能，我希望你能意识到使用 *Spring 方式* 编写有用的抽象并将对象创建委托给 Spring Python 容器，使得编写尚未成为此项目一部分的更多模块变得容易。

随着你的需求增长，该项目也会随之增长。Spring Python 使编写自己的模板和模块变得容易。你可以像 Spring Python 的预构建服务一样轻松地编写和注入自定义服务。如果你有关于新功能的想法，认为它们属于 Spring Python，你可以访问网站 http://springpython.webfactional.com 了解如何提交想法、补丁，或许加入团队帮助 Spring Python 发展。

## 安装 Spring Python

Spring Python 安装很容易。它附带两个可安装的 tar 包。一个是核心库本身。另一个是一组可选安装的示例，可帮助你更好地理解 Spring Python。由于它是用纯 Python 编写的，已安装的文件也是源代码，因此你可以了解 Spring Python 的工作原理。

![](img/bcf886d8b665372ee8afbb620d4273bb_31_0.png)

> 本安装部分不展示安装开发 Spring Python 项目补丁所需的所有部分。如需了解设置开发环境方面的更多信息，请加入 Spring Python 邮件列表 http://lists.springsource.com/listmanager/listinfo/springpython-users，在那里你可以询问当前开发要求。

## 为 Spring Python 设置环境

安装 Spring Python 目前需要 Python 2.4、2.5 或 2.6。它也可以在 Jython 2.5 上运行。在撰写本文时，由于缺乏向后兼容性和成熟度，它尚未扩展以支持 Python 3。然而，随着 Python 3 在用户社区中获得认可，Spring Python 也将迁移以支持它。

Spring Python 使与某些第三方库的集成变得更容易。本书不涵盖这些第三方库的安装。例如，要针对 Oracle 数据库编写查询，你需要安装 **cxora** 包。
