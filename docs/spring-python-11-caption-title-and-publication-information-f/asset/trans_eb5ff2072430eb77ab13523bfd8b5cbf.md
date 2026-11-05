
认证提供者预计要么成功要么失败，其行为如下：

- 如果认证 *成功*，返回一个认证对象。建议创建一个新的 `UsernamePasswordAuthenticationToken` 实例，将 `username`、`password` 和 `granted_auths` 复制进去，然后返回它。这是 Spring Python Security 的所有提供者都使用的同一个类。我们 *不* 需要在令牌上调用 `setAuthenticated(true)`。`AuthenticationManager` 会为我们处理
- 如果认证 *失败*，抛出一个 `BadCredentialsException`

对于此提供者，我们可以使用 `DatabaseTemplate` 来完成大部分工作。为此，我们需要一个注入的 SQL 连接工厂。如果我们使用用户提供的用户名和密码设置，则可以创建一个 `DatabaseTemplate` 并尝试查询以检索用户的角色。由于我们的 `DatabaseTemplate` 实例在执行查询之前会尝试登录数据库，因此这应作为我们的认证检查。如果成功，我们将在结果集中收到授予的权限。如果失败，我们将得到一个 SQL 异常，我们可以捕获它并替换为 `BadCredentialsException`。

```
from springpython.security.providers import *
from springpython.database.core import *

class MySqlLoginAuthenticationProvider(AuthenticationProvider):
    def __init__(self, factory=None):
        self.factory = factory

    def authenticate(self, authentication):
        self.factory.username = authentication.username
        self.factory.password = authentication.password
        dt = DatabaseTemplate(self.factory)

        try:
            authorities =
                dt.query("select authority from user where username = ?",
                        (authentication.username,),
                        rowhandler=DictionaryRowMapper())

            return UsernamePasswordAuthenticationToken(
                authentication.username,
                authentication.getCredentials(),
                [auth["authority"] for auth in authorities])
        except:
            raise BadCredentialsException("Invalid credentials")
```

由于每个连接工厂略有不同，我们假设传入的是一个 `MySqlConnectionFactory`。我们的新提供者可立即使用。就这么简单，满足了我们的需求：'users must be able to quickly write custom security extensions to handle legacy security solutions'。

使此示例更复杂的一种方法将是使其处理当前的一组连接工厂。但就目前而言，它作为演示如何在 Spring Python Security 当前未涵盖的方式下对现有系统进行身份验证的合适示例是有效的。

## 使用 Spring Python Security 的一些挑战

Spring Python Security — 如本章所示 — 极其灵活、高度可扩展且非侵入式。然而，这是有代价的。所有内容都必须手工配置（并且配置正确）。

这是针对 Acegi Security 提出的主要批评之一。此后，Acegi Security（现品牌为 Spring Security）发布了多个更新版本，减少了所需的手动配置量。他们现在为许多行业标准安全选项提供快捷方式，同时仍允许我们添加自定义选项，使为我们的需求[content]
认证提供者预计要么成功要么失败，其行为如下：

- 如果认证 *成功*，返回一个认证对象。建议创建一个新的 `UsernamePasswordAuthenticationToken` 实例，将 `username`、`password` 和 `granted_auths` 复制进去，然后返回它。这是 Spring Python Security 的所有提供者都使用的同一个类。我们 *不* 需要在令牌上调用 `setAuthenticated(true)`。`AuthenticationManager` 会为我们处理
- 如果认证 *失败*，抛出一个 `BadCredentialsException`

对于此提供者，我们可以使用 `DatabaseTemplate` 来完成大部分工作。为此，我们需要一个注入的 SQL 连接工厂。如果我们使用用户提供的用户名和密码设置，则可以创建一个 `DatabaseTemplate` 并尝试查询以检索用户的角色。由于我们的 `DatabaseTemplate` 实例在执行查询之前会尝试登录数据库，因此这应作为我们的认证检查。如果成功，我们将在结果集中收到授予的权限。如果失败，我们将得到一个 SQL 异常，我们可以捕获它并替换为 `BadCredentialsException`。

```
from springpython.security.providers import *
from springpython.database.core import *

class MySqlLoginAuthenticationProvider(AuthenticationProvider):
    def __init__(self, factory=None):
        self.factory = factory

    def authenticate(self, authentication):
        self.factory.username = authentication.username
        self.factory.password = authentication.password
        dt = DatabaseTemplate(self.factory)

        try:
            authorities =
                dt.query("select authority from user where username = ?",
                        (authentication.username,),
                        rowhandler=DictionaryRowMapper())

            return UsernamePasswordAuthenticationToken(
                authentication.username,
                authentication.getCredentials(),
                [auth["authority"] for auth in authorities])
        except:
            raise BadCredentialsException("Invalid credentials")
```

由于每个连接工厂略有不同，我们假设传入的是一个 `MySqlConnectionFactory`。我们的新提供者可立即使用。就这么简单，满足了我们的需求：'users must be able to quickly write custom security extensions to handle legacy security solutions'。

使此示例更复杂的一种方法将是使其处理当前的一组连接工厂。但就目前而言，它作为演示如何在 Spring Python Security 当前未涵盖的方式下对现有系统进行身份验证的合适示例是有效的。

## 使用 Spring Python Security 的一些挑战

Spring Python Security — 如本章所示 — 极其灵活、高度可扩展且非侵入式。然而，这是有代价的。所有内容都必须手工配置（并且配置正确）。

这是针对 Acegi Security 提出的主要批评之一。此后，Acegi Security（现品牌为 Spring Security）发布了多个更新版本，减少了所需的手动配置量。他们现在为许多行业标准安全选项提供快捷方式，同时仍允许我们添加自定义选项，使为我们的需求连接安全框架变得容易得多。

随着 Spring Python Security 的发展，它将做出类似的更改，同时增加其支持的协议。直到那时，使用尽可能多的自动化测试以确保配置按预期执行是良好的实践。对于问题和文档，请务必使用：

- 在线参考手册（`http://springpython.webfactional.com`）
- 支持论坛（`http://forum.springsource.org/forumdisplay.php?f=45`）
- 邮件列表（`http://lists.springsource.com/archives/springpython-users`）以获取最新答案

## 摘要

在本章中，我们深入探讨了复杂但功能强大的 Spring Python Security 架构的细节，并看到了其灵活性和可配置性。我们研究了将安全编码到应用程序中的挑战，并列出了一份有用的安全解决方案的需求清单。在整个章节中，我们看到了 Spring Python Security 如何满足这些需求。

我们开发了一个简单的应用程序，然后应用了简单、可硬编码的测试安全。后来，我们看到了由于 Spring Python 的 IoC 容器，可以轻松将其替换为 SQL 驱动的解决方案。我们还探索了配置 LDAP 解决方案。

我们通过了解如何轻松添加自己的自定义安全扩展，这些扩展可以轻松插入到 Spring Python Security 中来结束。

我们也坦率地观察到，使用 Spring Python Security 需要仔细配置。希望本书能降低门槛，使其更易于访问。

在本章中，我们涵盖了：

- 软件开发人员必须处理的安全问题以及有效编码解决方案非常具有挑战性
- 构建安全框架涉及许多需求，而 Spring Python 都满足了它们
- 我们用一个简单的解决方案包装了一个未受保护的应用程序，该解决方案通过委托给安全控制器来干净地保护应用程序
- 认证用户身份的概念，以及确定他们被授权执行的操作
- 测试应用程序的安全性是可能、实用且必要的
- 我们配置了一个基于 SQL 的安全系统，包括适应自定义用户/角色模式
- 我们配置了一个基于 LDAP 的安全系统
- 我们探索了使应用程序支持多个用户社区或在没有停机时间的情况下从一个安全系统迁移到另一个安全系统
- 我们研究了如何为 Spring Python Security 目前未开箱即支持的系统编写自己的安全扩展

在下一章中，我们将探索使用 Spring 的方式通过远程链接连接系统的方法。

## 7

## 使用 Spring Python 的远程处理跨节点扩展应用程序

随着近年来互联网在电子商务中的爆炸式增长，公司面临着支持大量同时客户的压力。随着用户希望拥有执行更多业务功能的更丰富界面，这不断导致对前所未有的更多计算能力的需求，无论是基于 Web 还是厚客户端。看到总 CPU 性能增长放缓，人们正在寻找多核 CPU、64 位芯片或向企业添加更多服务器以满足其不断增长的需求。

开发人员面临的挑战是在为节省成本而精简的桌面简单环境中设计应用程序。然后他们必须能够部署到多核、多服务器环境中，以满足公司的业务需求。

为了支持这一点，已经开发了不同的技术。已经起草了不同的协议来帮助节点之间进行通信。关于网络间通信是否应在 API 中可见或抽象掉的争论仍在继续。支持远程连接客户端进程与服务器进程的不同技术仍在不断开发中。

Spring Python 提供了一种干净的方式来采用简单的应用程序并使用远程处理技术将其拆分到多台机器上，可以无缝注入而不会导致代码重写头痛。Spring Python 使利用现有技术变得容易，同时也为支持尚未设计的技术做好准备。

在本章中我们将学习如何：

- Pyro 提供了一个不错的 Python 到 Python 的远程处理功能，可以轻松创建客户端-服务器应用程序
- Spring Python 与 Pyro 无缝集成，因此您的应用程序不必学习 API
- 您可以将简单应用程序转换为分布式应用程序，全部在同一台机器上
- 通过将应用程序拆分为部分，插入轮询队列管理器并运行服务器的多个副本而对业务逻辑没有影响，只需很少的精力即可重新布线应用程序

## Pyro（Python 远程对象）简介

Pyro 是一个开源项目（http://pyro.sourceforge.net），它提供了一种面向对象的 RPC 形式。如项目网站所述，它类似于 Java 的远程方法调用（RMI）。它与 CORBA（http://www.corba.org）不太相似，CORBA 是一种技术中立的线路协议，用于将多个进程连接在一起，因为它不需要接口定义语言，也不是面向将不同语言连接在一起的。Pyro 支持 Python 到 Python 的通信。得益于 Jython 的强大功能，轻松链接 Java 到 Python，反之亦然。

> Python Remote Objects 不应与 Python Robotics 开源项目（也名为 Pyro）混淆。

Pyro 在开箱即用的现有 Python 应用程序中非常易于使用。将发布服务的能力添加到现有应用程序并不困难。Pyro 使用自己的协议进行 RPC 通信。

从根本上说，基于 Pyro 的应用程序涉及启动 Pyro 守护程序线程，然后将服务器组件与此线程注册。从那时起，线程以及您的服务器代码处于待机模式，等待处理客户端调用。下一步涉及创建一个 Pyro 客户端代理，该代理配置为找到守护程序线程，然后将客户端调用转发到服务器。从高层次的角度来看，这与 Java RMI 和 CORBA 提供的类似。然而，由于 Python 的动态特性，配置步骤要容易得多，并且没有要求扩展任何类或实现任何接口。
