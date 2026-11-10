
假设有一个权限集（ACL）分配给一个对象，并且特定用户可以访问它。例如，人力资源部门的所有用户对其具有写入访问权限，而财务部门的用户具有读取权限。

如果我们为人力资源部和财务部的每个用户单独设计 ACL 以授予特定权限，我们就会陷入混乱。每当人力资源部或财务部有新用户加入时，我们就必须重新审视 ACL。

组帮我们避免了这种麻烦。不是为每个用户单独分配权限，而是将所有人力资源用户归为一个 HR 组，将所有财务用户归为一个 Finance 组，然后在组级别而不是针对特定用户分配权限。组可以分为两种类型：

- **公开：** 如果系统管理员或超级用户创建组，则该组被标记为公开。
- **私有：** 如果具有“Create group”权限的用户创建组，则该组被标记为私有。

在内部，Documentum 并不会对这两种类型的组进行区别对待。具体如何解释则取决于各个应用程序根据业务需求来决定。

组可以通过以下方式创建：

- DQL 查询
- API 命令
- Documentum Administrator 用户界面
- DFC 类

## 3.9 渲染

文档的渲染基本上是原始文档的表示，仅在格式上有所不同。Content Server 内部具有转换器来支持众多渲染。此外，将 Media Services 与 Content Server 集成还可以提供对富媒体格式的支持。

渲染的示例如下：

一名 Web 开发人员创建了一个内容模板，例如 create_article.xml，并将演示 XSL 文件附加到它，以便将其转换为 HTML 文件和 WML 文件。从此模板创建的内容将以 HTML 文件和 WML 文件的形式呈现，分别用于在网站和无线应用程序上显示。

HTML 和 WML 文件与原始内容文件仅在格式上不同，而内容相同。

图 3.6 以简化的方式解释了这一点。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_53_0.png)

Figure 3.6: 渲染

## 3.10 注册表

大多数应用程序维护一些应用程序表以存储特定于应用程序的数据。这些应用程序表可能包含 Documentum 客户端需要访问的记录。例如，Documentum 客户端应用程序中的下拉菜单可能需要从外部数据库表中显示世界上所有国家的列表，以便内容作者在创建内容时进行选择。

在注册之前，Content Server 不会知道外部数据库表。简而言之，注册表是 Content Server 所知的普通 RDBMS 表，尽管它们不是 Docbase 的一部分。

Documentum 为“Registered”表创建一个 dm_registered 对象，允许开发人员使用 DQL 查询从表中获取信息。

dm_registered 对象包含有关表名、表所有者、表列以及定义谁有权查询表并执行插入/更新/删除行等操作的信息。

我们将在第 11 章更详细地讨论注册表。

## 3.11 数据字典

Docbase 中的数据字典（DD）存储有关 Documentum 对象类型及其属性的信息。数据字典在以下场景中非常有用：

- **向用户显示的值帮助**
  示例：显示属性可能取值集合的下拉菜单
- **应用某些业务特定规则**
  示例：对属性值的约束，如“not null”和“unique key”

DD 可以支持多种语言环境，因此可以根据各种终端用户的默认语言环境对存储的信息进行本地化。此外，DD 信息由 Content Server 内部管理，一旦我们发布其中包含的数据，就可以将其提供给终端用户。

在撰写本书时，安装 Content Server 时 Documentum 提供的以下语言环境的数据字典信息为：

- 英语
- 法语
- 德语
- 意大利语
- 日语
- 韩语
- 西班牙语

## 3.12 方法和作业

方法是在 Docbase 中可执行的程序和脚本，可以由作业自动安排运行，作业是 Documentum 对象（dm_job），用于自动执行方法对象。以下各节将详细介绍方法和作业。

### 3.12.1 方法

方法脚本可以用 Java、Docbasic（Documentum 的专有语言）或其他语言编写。在 Documentum 中，方法被视为方法对象（dm_method），其属性定义了各种方法参数，例如执行代理、方法类型、超时时间和方法参数。方法可以由以下服务器执行代理执行：

- **Content Server：** 这是方法的默认执行代理，除非指定了特定的执行代理。Content Server 用于执行与 Docbasic 脚本或其他编程语言脚本相关的方法对象。
- **Method Server：** 这是一个守护进程，只要 Docbase 运行就一直运行，并由 Content Server 管理。注意，Method Server 默认随 Content Server 一起安装。Method Server 可用于执行与 Docbasic 脚本相关的方法对象。
- **Java 方法服务器：** 在 Content Server 安装期间，Documentum 在 Content Server 主机上将 Apache Tomcat 应用服务器安装为 Java 方法服务器。Java 方法服务器可用于执行调用 Java 方法的方法对象。

Documentum 方法可以在 Documentum Administrator 中的以下位置创建、修改和查看：Administration | Job Management | Methods。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_55_0.png)

Figure 3.7: wcmLifecycleMonitor 方法

图 3.7 显示了在 Documentum Administrator 中看到的示例方法 wcmLifecycleMonitor。

### 3.12.2 作业

无需手动或通过脚本调用方法，而是可以使用作业定期安排其执行。dm_job 对象属性定义作业参数，如关联方法、执行计划、方法参数等。

作业由随 Content Server 安装的 Documentum agent exec 进程执行。图 3.8 显示了作业和方法在 Documentum 架构中如何协同工作。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_55_1.png)

Figure 3.8: 作业和方法执行

Documentum 作业可以在 Documentum Administrator 中的以下位置创建、修改和查看：Administration | Job Management | Jobs。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_56_0.png)

Figure 3.9: Monitor_Lifecycles 作业

图 3.9 显示了在 Documentum Administrator 中看到的示例作业（Monitor_Lifecycles）。该作业进而调用图 3.10 中所示的方法 wcmLifecycleMonitor。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_56_1.png)

Figure 3.10: 'Monitor_Lifecycles' 作业调用的 'wcmLifecycleMonitor' 方法

## Documentum 5.3 更新

Documentum 5.3 版本引入了名为 Job Sequences 的新功能。这允许您配置多个作业按顺序（按指定顺序）在指定时间运行。

就目前而言，这就是我们希望在 Documentum 高级概念部分涵盖的全部内容。随着您阅读以下章节，您将更好地理解这些概念。

如果您对术语感到困惑或想再次复习概念，请将本章视为随时可查阅的参考。随时需要时都可以返回本章。

## 3.13 摘要

本章我们涉及了一些 Documentum 的高级概念。

我们讨论了 DMCL（Documentum 客户端库）作为客户端库，充当客户端与 Content Server 之间的 RPC 层。另一方面，DFC（Documentum 基础类）作为 DMCL 的包装器，公开 API 以执行基本内容管理功能。DFC 由一组 Java 接口和类组成，并内置了 Business Objects Framework（BOF），可帮助您通过扩展和自定义内容管理功能创建可重用的业务组件。

我们继续讨论了 Documentum WDK（Web 开发工具包），它作为一个框架，由多个服务器端可重用组件组成，使我们能够快速开发和部署需要与 Content Server 通信的基于 Web 的应用程序。

Webtop 是 Documentum 基于 WDK 提供的参考 Web 客户端实现。Web Publisher 是 Webtop 的扩展。它是一个基于 Web 浏览器的应用程序，为内容创建、审阅和发布到网站提供丰富的用户界面。

我们还讨论了访问控制列表（ACLs），它通过仅允许 Docbase 中特定组或用户拥有某些权限来帮助对 Documentum 对象施加安全限制。

别名集是 Documentum 系统不可或缺的一部分，定义了 Documentum 架构中各种实体的占位符，以便同一对象可以在多个上下文中重复使用。

我们了解了 Documentum 中组和用户如何被视为对象。用户是 Documentum 系统中一个或多个组的一部分。Documentum 中的渲染是与原始版本仅在格式上不同的文档。注册表是与 Docbase 注册的 RDBMS 表，以便可以在 Docbase 的上下文中使用这些数据中的数据。Documentum 中的数据字典是一个存储有关所有 Documentum 对象类型及其属性（属性）信息的存储库。最后，我们讨论了方法，即 Documentum Docbase 中的可执行程序，可以通过 Documentum 作业自动安排运行。
