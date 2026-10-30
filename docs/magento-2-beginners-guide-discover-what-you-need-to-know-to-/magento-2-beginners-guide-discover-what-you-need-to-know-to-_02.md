# 1 Magento 2 简介

在本章中，我们将涵盖以下主题：

- Magento 作为一种生活方式：Magento 作为平台和 Magento 社区
- 竞争对手：托管和自托管电子商务平台
- Magento 2 中的新特性
- 您需要什么来开始？

### Magento 作为一种生活方式

Magento 是一个开源电子商务平台。这是简短的定义，但我想结合我作为 Magento 生态系统一员七年的经历来定义 Magento。

在这七年中，Magento 不断发展至如今的形态——一个由热爱电子商务的人们支持的一站式解决方案。如果您选择 Magento 作为您电子商务网站的平台，您将定期收到平台更新。这些更新包括新功能、改进和错误修复，以增强您网站的整体体验。

作为 Magento 专家，我可以确认 Magento 是一个可以定制以满足任何需求的平台。这意味着您可以添加新功能、引入第三方库并定制 Magento 的默认行为。俗话说，唯一的限制就是您的想象力。

每当我要谈论 Magento 时，我总是会花一些时间谈论它的社区。Sherrie Rohde 是 Magento 社区经理，她分享了 2016 年 Magento 社区一些非常有趣的事实：

- 在 Magento 或围绕 Magento 的活动中发表了超过 725 场演讲
- 制作了超过 100 期关于 Magento 的播客节目
- 在超过 34 个国家组织和举办了会议及聚会小组
- 撰写了超过 1,000 篇关于 Magento 的博客文章

### 电子商务解决方案的类型

电子商务解决方案有两种类型：*托管*和*自托管*。

我们将分析每种电子商务解决方案类型，并涵盖每个类别中每个平台的一般信息、优点和缺点。

### 自托管电子商务解决方案

自托管电子商务解决方案是一种运行在您自己服务器上的平台，这意味着您可以下载代码、根据需求进行定制，然后将其部署在您喜欢的服务器上。Magento 就是一种自托管电子商务解决方案，这意味着您对 Magento 商店的定制和实施拥有绝对的控制权。

### WooCommerce

WooCommerce 是 WordPress 的一个免费购物车插件，可用于创建功能齐全的电子商务网站。WooCommerce 是按照 WordPress 的架构和标准创建的，这意味着您可以通过主题和插件进行定制。该插件目前已有超过 18,000,000 次下载，占所有在线商店的 39% 以上。

现在，我们来看看 WooCommerce 的优点和缺点：

优点：

- 可免费下载
- 易于安装和配置
- 有大量主题可用
- 市场中有近 400 个扩展
- 通过 WooCommerce 帮助台提供支持

缺点：

- WooCommerce 不能脱离 WordPress 使用
- 一些基本功能未包含在开箱即用中，例如将 PayPal 作为支付方式，这意味着您需要购买多个扩展来添加这些功能
- 通过扩展为 WooCommerce 添加自定义功能可能很昂贵

### PrestaShop

PrestaShop 是一个免费的开源电子商务平台。该平台目前被超过 250,000 家在线商店使用，并得到了超过 1,000,000 名成员社区的支持。PrestaShop 背后的公司提供一系列付费服务，例如技术支持、迁移和培训，用于运行、管理和维护商店。

现在，让我们看看 PrestaShop 的优点和缺点：

优点：

- 免费且开源
- 310 项集成功能
- 市场中有 3,500 个模块和模板
- 下载量超过 400 万次
- 支持 63 种语言

缺点：

- 与 WooCommerce 一样，许多基本功能默认不包含，通过扩展添加这些功能成本很高
- PrestaShop 社区中存在多个错误和抱怨

### OpenCart

OpenCart 是一个开源电子商务平台，在 GNU 通用公共许可证下可用。对于基础的电子商务网站来说，OpenCart 是一个不错的选择。

现在，让我们看看 OpenCart 的优点和缺点：

优点：

- 免费且开源
- 学习曲线平缓
- 有超过 13,000 个扩展可用
- 有超过 1,500 个主题可用

缺点：

- 功能有限
- 未针对 SEO 做好准备
- 管理面板中没有缓存管理页面
- 难以定制

### 托管电子商务解决方案

托管电子商务解决方案是一种运行在提供服务公司的服务器上的平台，这意味着该解决方案更容易设置，但存在限制，并且您没有根据需求自由定制解决方案的权利。当商店吸引更多流量并拥有更多客户和订单时，月费或年费会增加。

### Magento 2 中的新特性

Magento 2 是该平台的新一代版本，具有新功能、新技术和改进，使 Magento 成为目前最强大、最完整的电子商务解决方案之一。在本节中，我们将描述 Magento 1 和 Magento 2 之间的主要区别。

首先，让我们回顾一下 Magento 2 中可用的新技术：

### 新技术

- **Composer：** 这是一个 PHP 的依赖管理器。可以声明依赖关系，Composer 将通过安装和更新这些依赖来管理它们。在 Magento 2 中，Composer 简化了扩展的安装和升级以及 Magento 升级的过程。
- **Varnish 4：** 这是一个开源 HTTP 加速器。Varnish 将页面及其他资源存储在内存中，以减少响应时间和网络带宽消耗。
- **全页缓存：** 在 Magento 1 中，全页缓存仅在 Magento 企业版中提供。在 Magento 2 中，全页缓存已在所有版本中提供，允许缓存静态页面内容，从而提升性能并减少服务器负载。
- **Elasticsearch：** 这是一个搜索引擎，可改善 Magento 中的搜索质量，并提供后台重新索引和水平扩展。
- **RequireJS：** 这是一个按需加载 Javascript 文件的库，可减少 HTTP 请求数量并提高 Magento 商店的速度。
- **jQuery：** Magento 1 的前端使用 Prototype 作为 Javascript 语言。在 Magento 2 中，用于 Javascript 代码的语言是 jQuery。
- **Knockout.js：** 这是一个开源 Javascript 库，实现了**模型-视图-视图模型（MVVM）**模式，为创建交互式前端组件提供了一种极好的方式。
- **LESS：** 这是一个开源 CSS 预处理器，允许开发人员以更可维护和更可扩展的方式为商店编写样式。
- **Magento UI 库：** 这是一个模块化前端库，为通用元素使用一组 mixin，并允许开发人员更高效地处理前端任务。





现在，让我们回顾一下 Magento 2 中可用的新工具：

### 新工具

- **Magento Performance Toolkit**：这是一款工具，允许商家和开发者测试 Magento 安装及其定制的性能。
- **Magento 2 Command Line Tool**：这是一款工具，用于在 Magento 安装中运行一系列命令，以清除缓存、重新索引商店、创建数据库备份、启用维护模式等。
- **Data Migration Tool**：此工具允许开发者将现有数据从 Magento 1.x 迁移到 Magento 2。该工具包括验证、进度跟踪、日志记录和测试功能。
- **Code Migration Toolkit**：这允许开发者将 Magento 1.x 扩展和定制迁移到 Magento 2。需要手动验证和更新，以使 Magento 1.x 扩展与 Magento 2 兼容。
- **Magento 2 Developer Documentation**：Magento 社区的一个抱怨是 Magento 1 为开发者提供的文档不足。为了解决这个问题，Magento 团队创建了官方的 Magento 2 Developer Documentation，其中包含面向开发者、系统管理员、设计师和 QA 专家 的信息。

### 后台面板变更

- **Better UI**：后台面板拥有全新的外观，更加直观易用。此外，后台面板现在是响应式的，可以在任何设备、任何分辨率下查看。
- **Inline editing**：后台面板的网格支持内联编辑，以便更有效地管理数据。
- **Step-by-step product creation**：产品添加/编辑页面是后台面板中最重要的页面之一。Magento 团队在添加/编辑产品方面的体验上付出了大量努力，结果是您可以通过分步页面管理产品，该页面将字段和导入工具分在不同的部分。

### 前端变更

- **Integrated video in product page:** Magento 2 允许您为产品添加视频，引入了一种在目录中展示产品的新方式。
- **Simplified checkout:** 结账页面的步骤已被精简，以便客户在更短的时间内下单，从而提高 Magento 商店的转化率。
- **Register section removed from checkout page:** 在 Magento 1 中，客户有机会从结账页面的第一步注册。这要求客户在完成订单前考虑其账户和密码。为了简化结账流程，Magento 2 允许客户在订单成功页面注册，而不会延迟结账流程。

### 开始需要什么？

Magento 是一个非常强大的平台，总有新的东西需要学习。就在你以为你已经了解 Magento 的一切时，一个新版本就会发布，带来新的功能可供探索。这使得 Magento 作为电子商务平台变得有趣且独一无二。

话虽如此，本书将成为你的指南，帮助你发现实现、管理和维护你的第一个 Magento 商店所需了解的一切。

此外，我想强调一些在你掌握 Magento 之旅中会很有用的额外资源：

- **Official Magento Blog** (`https://magento.com/blog`)：获取 Magento 团队的最新消息：最佳实践、客户案例、与活动相关的信息以及 Magento 的一般新闻
- **Magento Resources Library** (`https://magento.com/resources`)：视频、网络研讨会和出版物，涵盖按类别组织的实用信息：订单管理、营销和商品管理、国际扩张、客户体验、移动架构和技术、性能和可扩展性、安全、支付和欺诈、零售创新以及业务灵活性
- **Magento Release Information** (`http://devdocs.magento.com/guides/v2.1/release-notes/bk-release-notes.html`)：这是您获取有关最新 Magento 版本的所有信息的地方，包括每个版本的亮点、安全增强、已知问题信息、新功能以及升级说明
- **Magento Security Center** (`https://magento.com/security`)：有关每个 Magento 安全补丁的信息，以及保持 Magento 商店安全的最佳实践和指南
- **Upcoming Events and Webinars** (`https://magento.com/events`)：官方即将举行的 Magento 活动列表，包括现场活动和网络研讨会
- **Official Magento Forums** (`https://community.magento.com`)：在官方 Magento 论坛上获取 Magento 社区的反馈

### Summary

在本章中，我们回顾了 Magento 2 以及在平台新版本中引入的变更。我们还分析了电子商务解决方案的类型以及最重要的可用平台。在下一章中，我们将回顾安装 Magento 2 以开始使用该平台的过程。
