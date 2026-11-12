## 最后考虑

本章标志着本书的结束，展示了未来可使用的未在示例应用程序中使用的其他组件的可能性。

在本章中，我们将概览本书早期章节示例应用程序中未使用的一些 Laminas 组件。我们将了解用于创建分布式和去中心化应用程序的组件，以及用于创建 API 的组件。最后，我们将了解在哪里可以获得 Laminas 组件的帮助，以及如何成为 Laminas 社区的一部分并改进 Laminas 框架。

本章将介绍以下主题：

- 其他 Laminas 组件
- 面向微服务的开发
- 构建 API
- 社区资源

### 其他 Laminas 组件

在本节中，我们将概览本书项目中未使用的一些 Laminas 组件。您可以在 `https://docs.laminas.dev/components` 阅读所有 Laminas 组件的文档。

### 缓存

让我们用旅行作为类比。我们在旅行中花费时间和金钱。既然时间就是金钱，我们在旅行中花费大量金钱。此外，我们走得越远，花费的金钱就越多。换句话说，最便宜的去处就是您当前所在的地方。原地停留是最便宜的旅行类型。这就是缓存的理念：将数据保存在尽可能最近的位置，以减少传输的时间和成本。`laminas-cache` 组件允许我们使用多个存储适配器缓存数据，这些适配器使您的应用程序能够从文件系统、Memcached、Redis 或 MongoDB 数据库等保存和读取数据。

### CLI

尽管我们使用 Laminas 开发 Web 应用程序，但该框架并不局限于此目的。事实上，PHP 手册指出 PHP 可以做任何事情。使用 Laminas，我们可以做任何事情，但控制力更强。

> **PHP 能做什么？**
> 您可以在 https://www.php.net/manual/en/intro-whatcando.php 找到 PHP 可能用途的描述。

信不信由你，您可以使用 Laminas 创建命令行 PHP 程序。为此，您应该使用 laminas-cli 组件。

### 加密

在我们的 whatstore 项目中，我们说过加密密码很重要。但您不应使用本书示例中使用的算法。建议使用强大的加密和密码哈希来保护和验证敏感数据。laminas-crypt 组件提供了一种在 PHP 应用程序中实现加密的简便且安全的方法。该组件由多位杰出的应用加密专家开发，例如 Enrico Zimuel，他是 PHP 框架互操作性组的核心成员。

### CSRF

实际上，没有名为 laminas-crsf 的组件。但是，有两个类处理 CSRF。CSRF 是跨站请求伪造的缩写，是一种攻击，有人试图通过伪造的表单发送恶意数据。laminas-form 组件提供 Element\Csrf 以向表单添加带超时配置的哈希元素。要验证哈希元素的值，可以使用 Laminas\Validator\Csrf 类。

### DOM

DOM 是文档对象模型的缩写。这是一个 Web 标准，文档位于 https://www.w3.org/DOM/DOMTR。根据该标准，HTML 标签可以作为对象层次结构来处理。此外，可以使用 XPath 语言和 CSS 选择器来搜索和选择对象。laminas-dom 组件使您可以使用这两种方法在 XML 和 HTML 文档中查找元素。自动化读取这些格式的文档非常有用，以便导入数据或通过实现网络爬虫在网站上搜索数据。





### XPath 和 CSS 选择器
XPath 语言的根文档可在 https://www.w3.org/TR/1999/REC-xpath-19991116 处查阅。CSS 选择器的根文档可在 https://www.w3.org/TR/2018/REC-selectors-3-20181106 处查阅。

### laminas-dom 的开发状态
`laminas-dom` 组件被认为功能完备，目前正处于 **仅安全** 维护模式，遵循 **Laminas 技术指导委员会** 的决定。这并不意味着该包已被弃用，而意味着只有安全更新会被纳入。然而，该委员会建议将 symfony/dom-crawler 作为替代组件，积极开发中（即功能不完整）。

### 如何处理被弃用的包
虽然 `laminas-dom` 未被弃用，但了解如果某个组件不再开发该怎么办是有用的。如果组件的维护者担心其用户，可以在组件的 GitHub 主页或其 Packagist 页面上找到关于替代组件的信息。

### Escaper
根据 **开放 Web 应用程序安全项目 (OWASP)**，注入是 2021 年十大 Web 应用程序安全风险之一。注入的一种形式是 **跨站脚本 (XSS)**。要防止 PHP 应用程序中此类注入攻击，可以使用 `laminas-escaper` 组件。此组件会中和带有恶意意图的注入代码，用于输出数据，无论代码是用 HTML、CSS 还是 JavaScript 编写的。

### OWASP 十大
OWASP 是一个旨在提高软件安全性的基金会。您可以在 https://owasp.org/www-project-top-ten 处找到安全风险、示例攻击场景及其预防方法的完整概述。

### Feed
如果您需要使用 RSS 或 Atom 源，可以使用 `laminas-feed` 组件。此组件允许您将馈源响应作为对象集合处理。

> **RSS 参考**
> 您可以在 https://validator.w3.org/feed/docs/rss2.html 处找到有关 RSS 的文档。

### Hydrator
在面向对象应用程序中，有一些常见操作，如下所示：

- 对象水合（用数据集填充它们）
- 从对象中提取数据

`laminas-hydrator` 组件提供水合和提取功能。此组件要求要进行水合和提取的类必须实现 `exchangeArray` 和 `getArrayCopy` 方法，与映射模型所需的方法相同，如我们在 *第 13 章，技巧和窍门* 中看到的那样。

### JSON-RPC 服务器
JSON 是 REST API 的事实上的标准。但这并非我的观点；而是巴拉吉·瓦拉纳西（Balaji Varanasi）和苏达·贝利达（Sudha Belida）在其书籍 *Spring REST* 中所述，该书由 *Apress* 出版于 2015 年。他们是对的。今天，除非有约束性要求，否则没有人会以 JSON 以外的格式公开 API 数据。

`laminas-json-server` 组件使您能够将 PHP 类公开为服务。使用 PHP DocBlocks，`laminas-json-server` 组件允许对 PHP 函数和方法进行远程过程调用。

> **JSON-RPC 参考**
> 您可以在 https://www.jsonrpc.org/specification 处找到 JSON-RPC 规范。

### LDAP
**轻量级目录访问协议** (**LDAP**) 是管理目录服务的开放标准协议。名为 OpenLDAP 的 LDAP 开源实现可在多种 Linux 发行版中找到。许多公司以 Red Hat 的 RHDS 或 Microsoft 的 Azure Active Directory 之名销售 LDAP 服务。您可以在大多数云服务提供商处找到 LDAP 实现。

拥有一个能简化在 PHP 应用程序中管理 LDAP 操作的组件是很有用的。这正是 `laminas-ldap` 所做的。

### LDAP 参考
您可以在 `https://ldap.com` 处找到更多有关 LDAP（包括其历史）的信息。另一方面，您可以在 `https://www.openldap.org` 处找到有关 OpenLDAP 及其实现的信息。

### 日志
您应该记得，我们多次使用 `error_log` 函数将消息保存到 Web 服务器日志中。在大多数情况下，我们使用此函数保存有关异常的消息。然而，曾有一段时间，我们使用 `error_log` 函数保存有关成功身份验证的信息。使用名称中带有 `error` 单词的函数来保存信息消息似乎不太合适，您同意吗？

要管理带有适当标识的日志消息并将消息存储到各种后端，可以使用 `laminas-log` 组件。此组件符合 PSR-3 规范，可在 `https://www.php-fig.org/psr/psr-3` 处查阅。

还有其他 Laminas 组件，但本节介绍的组件具有通用兴趣。

在下一节中，我们将讨论一个对分布式架构非常有用的组件。

### 面向微服务的开发
有一个特殊的组件，或者更准确地说，是一组 Laminas 组件，可以被视为微框架或微服务框架。我们指的是 **Mezzio**。

Mezzio 旨在根据 PHP-FIG 实现中间件概念。在 PSR-15 规范中，中间件“是一个单独的组件，通常与其他中间件组件一起，参与处理传入请求和创建结果响应” (`https://www.php-fig.org/psr/psr-15`)。使用 Mezzio，您可以轻松创建中间件管道来处理 HTTP 请求。

Mezzio 非常适合构建精简应用程序。此微框架基本上使用四个组件来创建中间件：路由器、依赖注入容器、模板引擎和错误处理程序。通过使用最少资源，Mezzio 帮助开发者在 PHP 中创建微服务。

您可以在 `https://docs.mezzio.dev` 处找到有关 Mezzio 的特定文档。

在下一节中，我们将讨论使用 Laminas 组件构建 API。事实上，我们已使用 Laminas 组件构建了 API。好消息是，现在我们将揭示有一个工具可以使此过程更轻松。

### 构建 API
Laminas 社区提供了一个用于构建 PHP API 的工具：**API 工具**。API 工具使用 Laminas 组件实现，但不要求开发者将 Laminas 作为框架使用。这意味着您可以使用 API 工具为使用任何库或框架编写的 PHP 应用程序生成 API。
您可以在 `https://api-tools.getlaminas.org` 处找到有关 API 工具的文档。

在下一节中，我们将介绍社区提供的一些有用的 Laminas 资源。

### 社区资源
除了 Laminas 文档之外，还有两个其他来源可用于了解 Laminas 和提问：

- **Laminas 论坛**：您无需在 `https://discourse.laminas.dev` 拥有帐户即可阅读官方 Laminas 论坛上的帖子。但是，您需要创建帐户才能发布问题。

- **Laminas GitHub 仓库**：Laminas 组件的开源代码也是文档。您可以在 `https://github.com/laminas` 处找到每个 Laminas 组件的仓库。此网页提供获取支持、为项目做出贡献和报告安全问题的基本指南。

此外，您可以在此处找到一些 Laminas 组件扩展示例：`https://github.com/fgsl/framework`。

### 摘要
在本章中，我们简要了解了一些未在本书示例应用程序中使用的 Laminas 组件。我们了解了一些与通用目的相关的组件，例如缓存和日志。

我们还了解到，有一个专门用于创建分布式和去中心化应用程序的组件，即 Mezzio 微框架。此外，我们了解到 Laminas 社区提供 API 工具来创建 PHP API。

最后，我们了解了在哪里可以获得 Laminas 方面的帮助，以及如何成为 Laminas 社区的一员并改进该框架。

真正的框架关注通用问题。但您应该利用 Laminas 的可扩展性和开源特性来实现特定问题的解决方案。

总而言之，记住敏捷宣言中的一个原则是件好事：“简洁——最大化未完成工作量的艺术——至关重要。” 使用 Laminas 并简化您的工作。

创造。只有存在的事物才能被改进。




