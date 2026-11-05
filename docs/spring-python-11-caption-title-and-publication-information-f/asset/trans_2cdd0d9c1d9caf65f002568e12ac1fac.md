
## 扩展我们的应用程序

本章开头，我们谈到了扩展应用程序的需求。到目前为止，我们展示了如何将一个非常简单的客户端-服务器应用程序拆分到两个 Python 实例之间。

然而，扩展应用程序通常意味着运行应用程序的多个副本以处理更大的负载。通常需要某种类型的负载均衡器。让我们探索如何使用 Spring Python 的远程服务结合一些简单的 Python 来创建服务的<|message|>多节点版本。

## 将单节点后端转换为多个实例

第一步是创建另一个应用程序上下文。我们将启动组件的不同配置，即不同的蓝图，并计划在不做任何更改的情况下重用现有的业务代码。

- 1. 创建一个双节点配置，该配置创建两个 Service 实例，并在不同的端口上进行广告。

```python
from springpython.config import *
from springpython.context import scope
from springpython.remoting.pyro import *

from simple_service import *

class HappyBirthdayContext (PythonConfig):
    def __init__(self):
        PythonConfig.__init__(self)

    @Object(scope.PROTOTYPE)
    def target_service(self):
        return Service()

    @Object()
    def service_exporter1(self):
        exporter = PyroServiceExporter()
        exporter.service_name = "service"
        exporter.service = self.target_service()
        exporter.service_port = 7001
        return exporter

    @Object()
    def service_exporter2(self):
        exporter = PyroServiceExporter()
        exporter.service_name = "service"
        exporter.service = self.target_service()
        exporter.service_port = 7002
        return exporter
```

2. 在此上下文中，我们有两个不同的 PyroServiceExporters，每个都有不同的端口号。虽然看起来它们都使用 target_service，请注意我们已将 scope 更改为 PROTOTYPE。这意味着每个导出器都有自己的实例。要运行此新上下文，我们需要一个不同的启动脚本。

```python
import logging
from springpython.context import *

from multi_server_ctx import *

if __name__ == "__main__":
    logger = logging.getLogger("springpython.remoting")
    loggingLevel = logging.DEBUG
    logger.setLevel(loggingLevel)
    ch = logging.StreamHandler()
    ch.setLevel(loggingLevel)
    formatter = logging.Formatter("%(asctime)s - %(name)s - %(levelname)s - %(message)s")
    ch.setFormatter(formatter)
    logger.addHandler(ch)

    ctx = ApplicationContext(HappyBirthdayContext())
```

3. 现在让我们启动脚本：

![](img/bcf886d8b665372ee8afbb620d4273bb_178_0.png)

在此脚本中，我们打开了 Spring Python 的一些内置日志记录，以便可以看到启动远程处理所涉及的部分。我们可以看到两个不同的 Pyro 守护线程被启动，每个端口一个。然后我们看到我们的 Service 代码的两个实例被注册，每个线程一个。

我们现在有两个服务副本已准备就绪，等待被调用。

## 创建轮询调度器

编写调度器的方式有很多。对于我们的示例，让我们选择一个简单的轮询算法，在其中循环遍历一个固定列表。

让我们创建一个应用程序上下文，其中包含一个轮询调度器，它的行为像我们的服务，并由 PyroProxyFactory 元素的列表提供。

```python
from springpython.config import *
from springpython.remoting.pyro import *

class RoundRobinDispatcher(object):
    def __init__(self, proxies):
        self.proxies = proxies
        self.counter = 0

    def happy_birthday(self, parm):
        self.counter += 1
        proxy = self.proxies[self.counter % 2]
        print "Calling %s" % proxy
        return proxy.happy_birthday(parm)

class HappyBirthdayContext(PythonConfig):
    def __init__(self):
        PythonConfig.__init__(self)

    @Object
    def service_proxies(self):
        proxies = []
        for port in ["7001", "7002"]:
            proxy = PyroProxyFactory()
            proxy.service_url = "PYROLOC://127.0.0.1:%s/service" % port
            proxies.append(proxy)
        return proxies

    @Object
    def service(self):
        return RoundRobinDispatcher(self.service_proxies())
```

我们将 service_proxies 配置为 PyroProxyFactorys 的数组。这使我们能够定义一个静态连接列表。

RoundRobinDispatcher 类被注入代理，并模仿我们的 Happy Birthday 服务的 API。调度器上的每次调用都会递增计数器。然后它选择其中一个代理来实际调用。

## 调整客户端配置，而无需客户端代码知道它正在与多节点后端通信

让我们编写客户端脚本以使用我们的轮询调度器。

```python
from springpython.context import *

from multi_client_ctx import *

if __name__ == "__main__":
    ctx = ApplicationContext(HappyBirthdayContext())
    s = ctx.get_object("service")

    print "\n".join(s.happy_birthday("Greg"))
    print "\n".join(s.happy_birthday("Greg"))
    print "\n".join(s.happy_birthday("Greg"))
```

对客户端几乎没有影响。唯一的区别是 import 语句，以及我们多次调用服务这一事实。如果客户端实际上是一个 GUI 应用程序，并且此操作绑定到按钮，则不会有任何区别。

现在让我们运行客户端脚本。

![](img/bcf886d8b665372ee8afbb620d4273bb_180_0.png)

如图所示，每次调用都会显示调用了哪个 PyroProxyFactory，并且它们清楚地显示在两个已注册的代理之间来回切换。随着每次调用与相应的 PyroServiceExporter 平稳集成，我们已将应用程序扩展为双节点版本。

## 摘要

此示例再次演示了如何利用 IoC 容器实现组件的复杂连接，而无需客户端或服务器知道它。

最新示例仅通过编写不同的蓝图集，即 IoC 配置，就完全重新布线了应用程序的原始概念。它还演示了使用纯 Python IoC 配置时事物更加强大和复杂。我们能够混合代码和参数，这比编写 XML 文件要顺畅得多。

我们的应用程序最初是一个简单的客户端调用服务。通过重新布线以使用 IoC，它使我们能够轻松重新配置以在多个节点上运行服务的多个副本，而无需更改核心逻辑。

我们的轮询调度器 admittedly 很简单。真正的生产解决方案需要：

- 更复杂的错误处理来处理诸如远程访问异常之类的事情。
- 我们还需要一种方法来扩展新节点并将它们添加到列表中。
- 也许通过让调度器自行导出，其他服务可以挂钩并找出有哪些服务可用。

![](img/bcf886d8b665372ee8afbb620d4273bb_181_0.png)

> Spring Python 不提供用于集成系统的调度器、路由器和其他类型的组件。Spring Integration (http://www.springsource.org/spring-integration) 是 Spring 组合的单独部分，为 Java 带来这些功能。Spring Python Remoting 是一个构建块，未来可用于构建 Spring Python Integration 模块。

我们可以花费无数小时来完善我们的调度器。我们也可以专注于为核心业务逻辑添加更多功能。

在所有这些中，重要因素在于我们已演示了 Spring Python 如何提供一种非侵入式的方式，将我们的原始应用程序扩展到远超其最初设计的目的。

在本章中我们学到了：

- Pyro 如何提供良好的 Python 到 Python 远程处理功能，以便轻松创建客户端-服务器应用程序
- Spring Python 如何与 Pyro 无缝集成，使您的应用程序不必学习 API
- 将简单应用程序转换为分布式应用程序是很容易的，所有这些都在同一台机器上
- 我们通过将应用程序拆分为部分、插入轮询队列管理器并在不影响业务逻辑的情况下运行服务器的多个副本来重新布线应用程序

在下一章中，我们将使用 Spring Python 的所有组件构建一个简单、可扩展且安全的应用程序，具有强大的完整性。
