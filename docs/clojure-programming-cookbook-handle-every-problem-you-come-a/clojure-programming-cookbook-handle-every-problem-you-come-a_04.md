# 8. Web 应用程序

## 引言

## Clojure 与 Vaadin - 简单的 Web 小部件

## 准备工作

## 项目设置

## Clojure/Vaadin 流程

## 如何操作...

## 使用日历小部件

## 对事件做出反应

## 点击按钮

## 使用和重新加载命名空间

## 服务器端推送

## 还有更多...

## 响应式 Vaadin - 思路

## 响应式 Vaadin - CPU 监控

## 响应式 Vaadin - Raspberry Pi 上的 Telegram

## 部署奖励

## 使用 Liberator 快速创建一个 REST API

## 准备工作

## 如何操作...

## 你的第一个 Liberator

## 使用 defresource

## 参数化资源

## 为 REST API 定义 GET/POST/PUT/DELETE 方法

## 如何工作...

## HTTP 的方法和状态码

## 追踪请求

## 还有更多...

## 使用 clojure.java.jdbc 的持久 REST 资源

## 为 jdbc 做准备

## 我们的服务规范

## 创建产品表并定义访问函数

## 定义资源并启动服务器

## 从 HTTP 客户端访问

## 使用 Immutant - 重用基础设施

## 准备工作

## 如何操作...

## 使用 Artemis 进行简单消息传递

## 远程消息传递

## 使用队列进行远程过程调用

## 调度代码执行

## 管道和定义数据转换管道

## 使用 WebSockets 变得简单

## 还有更多...

## 打包为独立版

## 打包为可部署的 Web 归档文件（WAR 文件）

## 使用 om.next 开发，使用下一代 ClojureScript 库

## 准备工作

## 为 om.next 创建新项目

## 更新 om 版本

## 依赖项提示

## 启动 figwheel

## 如何操作...

- 使用 om.next 入门
- 使用状态
- 如何工作...
- React 基础
- defui
- reconciler
- 组件生命周期
- 还有更多...
- 使用 om.next 的任务列表
- 定义命名空间和要使用的库
- 定义状态
- 定义 read
- 定义 mutate
- 定义 UI
- 9. 测试
- 引言
- 行为驱动开发
- 准备工作
- 依赖项
- 第一个特性
- 设计文件夹结构以组织特性
- 如何操作...
- 表格中的许多假设
- 从 CSV 文件读取假设
- 使用 Excel 实现
- 使用 Web API 和 MongoDB 实现
- 还有更多...
- VirtualBox
- Freactive
- Flambo，或 BDD 遇上 Apache Spark
- EEP 用于 BDD
- OpenCV
- 使用随机输入和基于模式的测试进行测试
- 准备工作
- 库
- 生成随机值或值组




- 基于模式（schema）的随机值
  - 如何实现...
  - 你的第一个 test.check 规范
  - 自动运行测试
  - 更多规范与生成器
  - 生成器与解构
  - Herbert
  - 更多内容...
- 使用 Criterium 进行基准测试、性能提示及其他工具
  - 准备工作
  - 如何实现...
  - 使用 Criterium
  - 工作原理...
  - 类型提示
  - 使用适当的数据类型
  - 映射、记录与类型的对比
  - 原始数组
  - 使用瞬态数据结构
  - 记忆化函数
  - 更多内容...
  - 使用 timbre 进行日志记录，使用 tufte 进行性能分析
  - 使用 jvisualvm
- 10. 部署与 DevOps
  - 简介
  - Riemann - 监控、事件投递与 Slack
    - 准备工作
    - 安装 Riemann 服务
    - 配置 Riemann 守护进程
    - Riemann 仪表盘
    - Riemann-health
    - 恰到好处的仪表盘配置
    - 准备发送事件
    - 如何实现...
    - 为服务进行基本事件上报
    - 事件过期
    - 工作原理...
    - 更多内容...
    - 从 Riemann 到 Slack webhooks
    - 从 Slack 机器人到 Riemann
  - 使用 Docker 部署 Clojure
    - 准备工作
    - 在 OS X 或 Windows 上安装 Docker
    - 在 Debian 上安装 Docker
    - 准备 Clojure 应用程序
    - 将应用程序打包为 JAR 文件
    - 创建 Docker 容器
    - 如何实现...
    - 直接连接到远程 Docker
    - 更多内容...
  - 在 Amazon Web Services 上使用 Clojure
    - 准备工作
    - 在 AWS 上注册
    - 获取访问密钥和秘密访问密钥
    - 在 project.clj 中设置依赖
    - 如何实现...
    - 使用 EC2
    - 使用 S3
    - 使用 Amazon SQS
  - 使用 AWS Lambda 实现无服务器 Clojure
    - Clojure 命名空间 helloworld
    - Hello World - AWS 部分
    - Hello Stream
    - 真实世界中的 Lambda
    - 工作原理...
    - 什么是 Amazon EC2？
    - Amazon S3
    - 存储桶和键
    - 对象
    - 没有更多了...

## Clojure 编程食谱

版权所有 © 2016 Packt Publishing

保留所有权利。未经出版商事先书面许可，本书的任何部分不得以任何形式或任何方式复制、存储于检索系统或传播，除非在批评性文章或评论中引用了简短引文。

在编写本书时，已尽一切努力确保所提供信息的准确性。但是，本书所包含的信息按“原样”销售，不作任何明示或暗示的担保。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已尽力通过适当使用大写字母来提供本书中提到的所有公司和产品的商标信息。但是，Packt Publishing 无法保证此信息的准确性。

首次出版：2016 年 10 月

生产参考号：1211016

由 Packt Publishing Ltd. 出版<br>
Livery Place<br>
35 Livery Street<br>
Birmingham<br>
B3 2PB, UK.

ISBN 978-1-78588-503-7

## 贡献者名单

| 角色 | 姓名 |
|---|---|
| 作者 | Makoto Hashimoto, Nicolas Modrzyk |
| 文字编辑 | Safis Editing, Sameen Siddiqui |
| 审稿人 | Eduard Bondarenko, Dmitri Sotnikov |
| 项目协调员 | Ritika Manoj |
| 策划编辑 | Kartikey Pandey |
| 校对员 | Safis Editing |
| 选题编辑 | Kirk D'costa |
| 索引编制员 | Tejal Daruwale Soni |
| 内容开发编辑 | Sachin Karnani |
| 美术设计 | Abhinash Sahu |
| 技术编辑 | Pranav Kukreti |
| 制作协调员 | Aparna Bhagat |

## 关于作者

**Makoto Hashimoto** 是日本的一名技术爱好者。作为 IT 架构师，他参与了许多项目，并以他的技术领导力成功地领导了这些项目。Makoto 热爱编程，尤其是函数式编程语言。因此，自从第一次接触 Clojure 以来，他就成为了 Clojure 的爱好者。最近，他经常参与大数据领域的项目，并不断思考将 Clojure 用于该领域的新方法。他喜欢在周末与家人共度时光，此外他还喜欢烹饪、古典音乐、艺术和体育。

Makoto 还用日语写了两本书。你可以在 http://amzn.to/2e0jySv 和 http://amzn.to/2eg3gq8 找到它们。

> *致我的合著者 Nicolas，与你共事是我的莫大荣幸。非常感谢你的宝贵建议和鼓励。致我的家人，我要感谢我的家人和朋友们的持续鼓励和支持。没有你们，我不可能完成这项工作。*

**Nicolas Modrzyk** 在 IT 领域拥有多年经验，并在亚洲生活多年。他喜欢将想法变为现实，而且这个现实能将世界各地的人们聚集在一起。他参与了许多 IT 项目，帮助客户达成目标，并普遍尝试让 IT 更容易被每个人掌握。

五年前，Nicolas 一头扎进了 Clojure 的世界；这帮助他变得更加专注，不再把时间浪费在不值得的事情上。Clojure 不仅仅是一种神奇的程序设计语言；它是一种充满冒险的生活。希望这本书能带你走上那段冒险之路。

Nicolas 还用日语写了一本书《*Oishii Clojure*》（http://gihyo.jp/book/2013/978-4-7741-5991-1），其中包含大量简短的 Clojure 菜谱，让你享受 Clojure 语言，并激发你尝试新事物的欲望。

感谢你，Makoto，主动联系并让这本书成为一段奇妙的冒险！同时，我也期待我们的下一本书！致我的女儿们，Mei 和 Manon，我始终想念着你们，即使是在那些为这本书工作的深夜。致我亲爱的家人和了不起的朋友们，感谢你们一直以来的支持，即使在那些我满口只谈 Clojure 的时刻。

## 关于审稿人

**Eduard Bondarenko** 是一位长期使用 Ruby 和 Clojure 的程序员。他最近开始涉足 Android 开发。他偏爱简洁、富有表现力且带有少量注释的代码，并且尝试过许多编程语言，如 Python、Erlang、Node.js、Elm、Scala 等。Eduard 曾审阅过几本 Clojure 书籍，他喜欢所有这些书，因为它们主题有趣且广泛。除了编程，他喜欢与家人共度时光、踢足球和旅行。

> 我想感谢我的家人对我参与这本书的愿望的支持，也感谢作者们展示了 Clojure 各种非凡的应用。

## www.PacktPub.com

### 电子书、折扣优惠及更多

您是否知道 Packt 为每一本已出版的书提供电子书版本，并提供 PDF 和 ePub 文件？您可以在 www.PacktPub.com 升级到电子书版本，作为纸质书客户，您有权获得电子书折扣。如需更多详情，请通过 customercare@packtpub.com 与我们联系。

在 www.PacktPub.com，您还可以阅读免费的技术文章合集，订阅一系列免费的新闻通讯，并获得 Packt 图书和电子书的专属折扣和优惠。

![](img/d7b631352a782ee50160cf8eb995152f_31_0.png)

https://www.packtpub.com/mapt

通过 Mapt 获取最热门的软件技能。Mapt 让您完全访问所有 Packt 图书和视频课程，以及行业领先的工具，帮助您规划个人发展并推进职业生涯。

### 为什么订阅？

- 可对 Packt 出版的每本书进行全文搜索
- 可复制、粘贴、打印和标记内容
- 按需提供，可通过网页浏览器访问

## 前言

这本书旨在激发你尝试新事物的欲望。它将带你回到那些充满活力、准备好马上行动并让事情发生的日子。

Clojure 是一种独一无二的编程语言，其核心非常重要，你可以基于强大的构建模块写出高效的代码。

随着 Clojure 1.9 即将发布，这种语言以其简单和简洁而领先于其他语言。因此，无论你是在用树莓派开展周末编程项目，还是准备以你的新创业公司挑战世界，本书中提供的众多优秀菜谱都适合你。

本书大部分内容可以按任意顺序阅读，但请务必先阅读前几篇菜谱，以回顾 Clojure 的基础知识。

让事情发生，并享受其中的每一部分。

### 本书涵盖的内容

- 第 1 章，*使用 Clojure 进行实时编程*，帮助你回顾 Clojure 基础如何与不同的数据类型协同工作、代码控制流如何运作，以及如何集成第三方库。你可能已经知道这些，但一本好的烹饪书如果没有坚实的基础是无法开始的。

- 第 2 章，*与集合交互*，涵盖选择、过滤、转换、合并、连接，以及如何变得懒惰。懒惰是 Clojure 最伟大的特性之一，我们希望确保你始终足够懒惰。

- 第 3 章，*下一代 Clojure*，向你介绍宏、高级宏、转换器（转换器到底是什么？）以及逻辑编程。它



涵盖了 Clojure 数据作为代码特性的预处理和后处理。

第 4 章，*文件访问与网络*，涵盖了 StreamingQueues 和实时网络，这让你有能力思考分布式系统以及如何连接你的物联网设备。它还涵盖了文件以及如何操作文件来帮助你进行轻量级处理以实现互联。

第 5 章，*与其他语言协同工作*，向你介绍了基本的 Clojure 工具，使你能够轻松集成用其他语言编写的代码，例如 Java、Scala 和 .NET，一直到在 JavaScript 运行时上的 Clojure，ClojureScript。

第 6 章，*并发与并行*，帮助你学习在 Clojure 进程之间分配任务的多种方式，与类似 Scala 的 actors 交互，并共享不同实例之间的状态。当然，本章还涵盖了在 Clojure 中编写并行代码的最简单方法，core.async。

第 7 章，*高级技巧*，帮助你了解如何不仅进行结对编程，还进行共享实时结对编程。你还将看到如何自行 hack Clojure 源代码。

第 8 章，*Web 应用*，带你了解 Web 应用、Web 服务或 API 端点或你称之为的任何东西。你还将学习如何使用 Vaadin 的漂亮小部件，如何使用 Liberator 编写类似 REST 的 API，以及使用 Immutant，这是一组位于你 Clojure 代码指尖的服务器端服务。

第 9 章，*测试*，不仅帮助你验证代码，还帮助你使用随机模式测试来处理你未曾考虑过的案例。本章还介绍了 Clojure 中的行为驱动开发以及一些 OpenCV 验证，最后，解释了如何基准测试你的代码速度。

第 10 章，*部署与 DevOps*，涵盖将你的 Clojure 代码 docker 化，使其在本地运行就像在云中一样，为你的关键服务部署实时监控，最后，你喜欢的 Amazon 技巧也在这里涵盖。

## 本书所需

你需要一点时间、一台电脑和一个文本编辑器。这就是本书所需的一切。当然，之前的函数式编程经验是很大的加分项，但拥有动力和精力几乎就是你所需要的一切。

## 本书适合谁

本书适合想要让事情发生的人。沿着本书中包含的无数代码示例的思路跟随，然后继续前进。每个示例都应该让你想要尝试周围的东西，并产生一些新想法。本书一开始可能只是一本食谱书，但到最后，将成为你的食谱书。

## 章节

在本書中，你会发现几个经常出现的标题（准备就绪、如何操作、其工作原理、更多内容、另请参阅）。

为了清晰地说明如何完成一个配方，我们使用这些部分如下：

### Getting ready

本节告诉你在这个配方中会有什么，并描述如何设置配方所需的任何软件或任何初步设置。

### How to do it...

本节包含遵循配方所需的步骤。

### How it works...

本节通常包含对上一节中发生的事情的详细解释。

### There's more...

本节包含有关配方的附加信息，以便让读者更了解配方。

### See also

本节提供指向其他有用信息的帮助链接，供配方使用。

## 约定

在本書中，你会发现一些区分不同类型信息的文本样式。以下是这些样式的一些示例及其含义。

代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下："However, declaring libraries in `project.clj` is much simpler than doing it in the `pom.xml` file of Maven."

代码块设置如下：

```
String str = "Hello ".concat("world !");
```

任何命令行输入或输出写成如下：

```
$ mv lein ~/bin
```

新术语和**重要词语**以粗体显示。你在屏幕上看到的词语，例如在菜单或对话框中，出现在文本中如下："Go to the **Data Buckets** tab and click on the **Create New Data Bucket** option."

### Note

警告或重要说明以如下框显示。

### Tip

提示和技巧如下显示。

## 读者反馈

我们始终欢迎读者的反馈。请让我们知道你对这本书的看法——你喜欢或不喜欢什么。读者反馈对我们非常重要，因为它帮助我们开发你真正能从中获益的书名。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及书名即可。

如果你在某个主题上有专业知识，并且有兴趣撰写或为一本书做出贡献，请参阅我们的作者指南，网址为 www.packtpub.com/authors 。

## 客户支持

现在你已成为 Packt 书籍的自豪拥有者，我们有许多事情可以帮助你充分利用你的购买。

## 下载示例代码

你可以从 http://www.packtpub.com 在你的账户中下载本书的示例代码文件。如果你是在其他地方购买此书，你可以访问 http://www.packtpub.com/support 并注册以便将文件直接发送到你的电子邮件。

你可以通过以下步骤下载代码文件：

- 1. 使用你的电子邮件地址和密码登录或注册我们的网站。
- 2. 将鼠标指针悬停在顶部的 SUPPORT 选项卡上。
- 3. 点击 Code Downloads & Errata。
- 4. 在搜索框中输入书名。
- 5. 选择你想要下载代码文件的书籍。
- 6. 从下拉菜单中选择你购买此书的地点。
- 7. 点击 Code Download。

你也可以通过在 Packt Publishing 网站的书籍网页上点击 Code Files 按钮来下载代码文件。此页面可以通过在搜索框中输入书名来访问。请注意，你需要登录你的 Packt 账户。

下载文件后，请确保使用最新版本的以下工具解压或提取文件夹：

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

本书的代码包也托管在 GitHub 上，地址为

https://github.com/PacktPublishing/Clojure-Programming-Cookbook。我们还有其他代码包来自我们丰富的图书和视频目录，可在 https://github.com/PacktPublishing/ 获取。快来看看吧！

## 下载本书的彩色图片

我们还为你提供了一个 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。彩色图片将帮助你更好地理解输出中的变化。你可以从 https://www.packtpub.com/sites/default/files/downloads/ClojureProgram 下载此文件

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果你发现我们的书籍中存在错误——可能是文本或代码中的错误——我们将非常感激你向我们报告。通过这样做，你可以拯救其他读者免受挫折，并帮助我们改进本书的后续版本。如果你发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择你的书籍，点击 Errata Submission Form 链接，并输入你的勘误详情。一旦你的勘误得到验证，你的提交将被接受，勘误将被上传到我们的网站或添加到该标题现有勘误列表的 Errata 部分。

要查看之前提交的勘误，请前往 https://www.packtpub.com/books/content/support 并在搜索字段中输入书名。所需信息将出现在 Errata 部分下。

## 盗版

互联网上版权材料的盗版是一个持续存在的问题，涉及所有媒体。在 Packt，我们非常重视对版权和许可的保护。如果你遇到我们



在互联网上以任何形式传播，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上涉嫌盗版材料的链接。

我们感谢您帮助保护我们的作者以及我们为您提供有价值内容的[content]

在互联网上以任何形式传播，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上涉嫌盗版材料的链接。

我们感谢您帮助保护我们的作者以及我们为您提供有价值内容的能力。

## 问题

如果您对本书的任何方面有问题，可以联系我们 questions@packtpub.com，我们会尽力解决您的问题。
