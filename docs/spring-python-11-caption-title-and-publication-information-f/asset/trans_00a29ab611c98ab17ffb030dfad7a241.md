
## 8

## 案例研究 I—将 Spring Python 与您的 Web 应用程序集成

在之前的章节中，我们已经介绍了 Spring Python 的构建模块：依赖注入、面向方面编程、数据库模板、事务管理、安全性和远程调用。所有这些组件就像建造房屋时使用的砖块。我们单独查看了每块砖，并了解了如何利用它。在本章中，我们将探讨如何将它们全部结合起来构建一个全面的银行应用程序。

本章我们将学习如何：

- 组装一个简单的银行应用程序，具有良好解耦的视图和控制器层
- 应用简单的身份验证机制以授予不同类型用户访问权限
- 应用基于角色的授权，区分不同的用户组
- 创建自定义授权以防止客户查看彼此的数据
- 通过受信任的网络以原始的机器可读格式导出数据
- 向外部用户导出数据，通过既定的安全协议仅向经过身份验证和授权的客户端提供此数据
- 无缝审计银行操作
- 将多步骤操作标记为原子事务

## 一个优秀银行的需求

在开始构建应用程序之前，我们需要确定在编码冲刺中将实现的故事。我们需要做什么才能实现一个良好的银行应用程序？

- 客户可以开设余额为 $0.00 的新账户
- 客户可以关闭余额为 $0.00 的账户
- 开户和销户记录写入日志，账户所有者和任何经理均可查看
- 客户可以提取账户总额范围内的任意金额
- 客户可以向其拥有的现有账户存入任意金额
- 客户可以将其拥有的一个账户向其拥有的另一个账户转账，金额不超过源账户总额
- 所有取款、存款和转账均写入日志，账户所有者和任何经理均可查看
- 日志将通过安全的机器对机器格式提供，需要有效凭据
- 经理查看日志的操作将单独记录。此日志可由主管查看

这并不是我们希望银行具备的全部功能，但这是一个不错的开始。我们的冲刺专注于构建一些基本的存款/取款功能，同时记录这些交易。这要求保持完整性以避免资金泄露。

我们还需要对经理进行监督，以监控他们何时检查交易日志。客户、经理和主管的访问将需要在我们的银行应用程序中设置一些细粒度的安全控制。

最后，通过远程连接读取日志的能力将很好地支持与其他银行的集成，而我们的客户可能与这些银行合作。

开始吧！

## 构建骨架 Web 应用程序

为了开始，我们需要一个 Web 应用程序的外壳，以便我们可以开始填充细节。我们之前使用 CherryPy (http://cherrypy.org) 构建了一个简单的 Web 应用，因此让我们用它来启动。

1. 首先，让我们编写一个主应用程序来引导 CherryPy 以及我们的应用程序上下文。

```
import cherrypy
import os
import ctx
from springpython.context import ApplicationContext

if __name__ == '__main__':
    cherrypy.config.update({'server.socket_port': 8009})

    applicationContext =
        ApplicationContext(ctx.SpringBankAppContext())

    cherrypy.tree.mount(
        applicationContext.get_object("view"),
        '/',
        config=None)

    cherrypy.engine.start()
    cherrypy.engine.block()
```

你可能会注意到，它在包 ctx 中查找 SpringBankAppContext 以找到视图对象。

2. 让我们编写一个简单的纯 Python IoC 容器来创建我们需要的 CherryPy 视图对象。

```
from springpython.config import PythonConfig, Object

from app import *

class SpringBankAppContext(PythonConfig):
    def __init__(self):
        PythonConfig.__init__(self)

    @Object
    def view(self):
        return SpringBankView()
```

这是最简单的容器。我们基本上只有一个对象 view，它返回 SpringBankView 的一个实例。以后如果需要其他注入对象或功能，我们可以轻松更新此配置。

3. 现在让我们编写一个简单的视图层，具有 Hello, world 风格的打开页面。

```
import cherrypy

class SpringBankView(object):

    @cherrypy.expose
    def index(self, args=None):
        return """
            Welcome to SpringBank!
        """
```

4. 让我们启动应用程序看看它是什么样子。

![](img/bcf886d8b665372ee8afbb620d4273bb_186_0.png)

5. 现在让我们访问 http://127.0.0.1:8009 的广告位置。

![](img/bcf886d8b665372ee8afbb620d4273bb_186_1.png)

这是我们 Web 应用程序的起点。从这里开始，我们将在本章剩余部分处理我们刚刚描述的功能。

## 保护应用程序

在开始之前，让我们先插入一些安全功能。在安全章节中，我们讨论了安全功能如何方便地在事后添加。但如果一开始就先实现会更好。我们将通过添加一个简单的登录页面并硬编码三个账户来实现：客户、银行经理和银行主管。

1. 让我们修改引导主程序，添加一些额外功能以启用安全。

```
import cherrypy
import os
from springpython.context import ApplicationContext
from springpython.security.context import *

from ctx2 import *

if __name__ == '__main__':
    cherrypy.config.update({'server.socket_port': 8009})

    ctx = ApplicationContext(SpringBankAppContext())

    SecurityContextHolder.setStrategy(SecurityContextHolder.MODE_GLOBAL)
    SecurityContextHolder.getContext()

    conf = {"/": {"tools.sessions.on":True,
                  "tools.filterChainProxy.on":True}}

    cherrypy.tree.mount(
        ctx.get_object("view"),
        '/',
        config=conf)

    cherrypy.engine.start()
    cherrypy.engine.block()
```

首先，我们需要初始化存储在 SecurityContextHolder 中的安全上下文设置。CherryPy 要求的另一件事是打开 HTTP 会话。我们还必须激活 filterChainProxy。Spring Python 的 FilterChainProxy 在我们的应用程序上下文中设置后，会自动注册为 CherryPy 工具。

2. 让我们将安全组件添加到我们的 IoC 配置中。

```python
from springpython.config import PythonConfig, Object
from springpython.security.providers import *
from springpython.security.providers.dao import *
from springpython.security.userdetails import *
from springpython.security.vote import *
from springpython.security.web import *
from springpython.security.cherrypy3 import *

from app2 import *

class SpringBankAppContext(PythonConfig):
    def __init__(self):
        PythonConfig.__init__(self)

    @Object
    def view(self):
        view = SpringBankView()
        view.auth_provider = self.auth_provider()
        view.filter = self.auth_processing_filter()
        view.http_context_filter = self.
        httpSessionContextIntegrationFilter()
        return view

    @Object
    def filterChainProxy(self):
        return CP3FilterChainProxy(filterInvocationDefinitionSource =
            [
            ("/login.*",  ["httpSessionContextIntegrationFilter"]),
            ("/.*",
                ["httpSessionContextIntegrationFilter",
                "exception_translation_filter",
                "auth_processing_filter",
                "filter_security_interceptor"])
            ])
```

@Object
def httpSessionContextIntegrationFilter(self):
    filter = HttpSessionContextIntegrationFilter()
    filter.sessionStrategy = self.session_strategy()
    return filter

@Object
def session_strategy(self):
    return CP3SessionStrategy()

@Object
def exception_translation_filter(self):
    filter = ExceptionTranslationFilter()
    filter.authenticationEntryPoint = self.auth_filter_entry_pt()
    filter.accessDeniedHandler = self.accessDeniedHandler()
    return filter

@Object
def auth_filter_entry_pt(self):
    filter = AuthenticationProcessingFilterEntryPoint()
    filter.loginFormUrl = "/login"
    filter.redirectStrategy = self.redirectStrategy()
    return filter

@Object
def accessDeniedHandler(self):
    handler = SimpleAccessDeniedHandler()
    handler.errorPage = "/accessDenied"
    handler.redirectStrategy = self.redirectStrategy()
    return handler

@Object
def redirectStrategy(self):
    return CP3RedirectStrategy()

@Object
def auth_processing_filter(self):
    filter = AuthenticationProcessingFilter()
    filter.auth_manager = self.auth_manager()
    filter.alwaysReauthenticate = False
