
# Force.com Enterprise Architecture
# 第二版

构建并交付满足企业业务需求的打包式 Force.com 应用程序

作者：Andrew Fawcett

Packt Publishing www.packtpub.com

伯明翰 - 孟买

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_0_0.png)

版权所有 © 2017 Packt Publishing

版权所有。未经出版事先书面许可，本书任何部分不得以任何形式或任何方式复制、存储在检索系统中或传输，除非用于批判性文章或评论中的简短引用。

在本书准备过程中，已尽全力确保所呈现信息的准确性。但是，本书所包含的信息不附带任何明示或暗示的担保而出售。作者、Packt Publishing 及其经销商和分销商对本书直接或间接造成或被指控造成的任何损害概不负责。

Packt Publishing 已努力通过适当使用大写字母提供本书中提及的所有公司和产品的商标信息。但是，Packt Publishing 不能保证此信息的准确性。

首次出版：2014 年 9 月
第二版：2017 年 3 月

制作参考编号：1280317

由 Packt Publishing Ltd. 出版。
Livery Place
35 Livery Street
英国伯明翰 B3 2PB

ISBN 978-1-78646-368-5

www.packtpub.com

## 致谢

- **作者**
  Andrew Fawcett

- **项目协调员**
  Vaidehi Sawant

- **审稿人**
  Zarna Chintan Naik
  John M. Daniels
  Peter Knolle
  John Leen
  Aaron Slettehaugh
  Avrom Roy-Faderman

- **校对**
  Safis Editing

- **索引编制**
  Mariammal Chettiyar

- **图形**
  Jason Monteiro

- **委托编辑**
  Aaron Lazar

- **制作协调员**
  Shraddha Falebhai

- **获取编辑**
  Nitin Dasan

- **封面设计**
  Shraddha Falebhai

- **内容开发编辑**
  Rohit Kumar Singh

- **技术编辑**
  Pavan Ramchandani
  Kunal Chaudhari

- **文案编辑**
  Sonia Mathur
  Pranjali Chury

## 前言

作为 Salesforce.com 的开发者布道师，我一直看到对 Andrew 用这本书带来的这类材料的需求与日俱增。虽然我经常有幸向开发者展示 Force.com 上即将推出的新功能，但我也经常被问及如何利用这些功能并在企业开发者团队所需的生态系统中实施解决方案时的日常挑战。本书将在为这些特定顾虑提供新参考材料方面大有裨益。

2007 年，我开始在 Salesforce.com 上进行开发，当时的面貌与今天大不相同。那时没有 Visualforce。Apex 刚刚推出，缺少许多现在拥有的接口，例如 batchable 和 schedulable。大多数自定义界面是通过在 S-Controls 中嵌入大量 JavaScript 来实现的，S-Controls 是平台不再支持的部分。与平台的通信可以通过使用 AJAX 工具包的 SOAP API 完成。如果你想在服务器端拥有真正精细的业务逻辑，可以通过在 Apex 中自定义 SOAP 端点来公开它。

2014 年，平台的功能已远超那些旧日时光。借助标准控制器，Visualforce 开箱即用，无需额外代码即可提供基本业务逻辑。对于自定义处理，开发者不再局限于单一选项，而是有多种途径，从扩展 Visualforce 组件库到直接将 Apex 方法暴露给 JavaScript——这提供了旧 AJAX 工具包的灵活性，而无需访问平台 API 的范围。Apex 可以被调度，可以在后台处理记录，也可以用于创建完全自定义的 REST 端点。开发者现在可以使用 Streaming 和 Analytics 等强大的新 API，以及工业级身份服务。

平台仍在不断发展。每年在 Dreamforce 上，我们都会向平台宣布新工具、新功能和新特性。去年是 Salesforce1，随附一个新的移动应用程序，使将界面部署到智能手机变得简单且集成。即将到来的十月，我们将推出具有变革意义的创新。

技术演进的这种步伐，加上 Force.com 在企业应用中日益广泛的采用，为开发者带来了一个特定的挑战：不断地将平台视为各种用例的解决方案之外的东西，而是将其视为高效使用平台的完整生态系统。仅仅认为某个应用在平台上运行已不再足够；开发者需要考虑他们的应用程序是否以一种利用正确功能并能高效、良好共存的方式进行设计。需要有能力从高层次并带有清晰方向地审视平台的局限性。

当我们开始讨论 Andrew 基于 Martin Fowler 的关注点分离（Separation of Concerns）编写的一系列新文章，以及这些设计模式如何用于为企业解决方案开发 Apex 时，我知道 Andrew 正是具备这种能力的架构师。七年前，以这种抽象层次思考 Apex 当然是可能的——只不过并非真正必要。如今，Force.com 开发者手握所有潜在的工具和功能，而不考虑这些概念，无疑是在为未来埋下维护债务的隐患。

对熟悉其公司工作的人来说，Andrew 有条件撰写这本书并不令人惊讶。FinancialForce.com 创建了我在 Force.com 上见过的最强大的应用程序之一，作为首席技术官，Andrew 一直处于使其成功的前沿。

因此，我很高兴看到 Andrew 撰写这本书，并且从其核心来看，我们可以看到他之前设计模式文章的扩展版本。实际上，仅仅印刷那些文章的副本也不会成为架构师图书馆中不好的补充，但在这里，我们还看到了一个更完整的愿景，即开发者在构建平台上的应用程序之前应该了解的内容，这一愿景从更高阶的考虑（例如将 Apex 类相互接口）一直延伸到具体任务（例如正确利用 Force.com 的源代码控制软件）。

很高兴看到这本书在我的书架上，希望也在你的书架上——它将帮助你不仅规划这一代 Force.com 应用程序，还能为未来的应用程序向前发展。

Joshua Birk
Salesforce 开发者布道师

## 关于作者

**Andrew Fawcett** 拥有超过 25 年的经验，多年来曾担任多个软件开发相关职位，越来越专注于与主要国际会计和 ERP 软件供应商的企业级产品架构，并承担越来越大的责任。他精通在各种技术平台、框架、行业设计模式和方法论中执行和管理软件开发生命周期的各个方面，最近主要涉及 Salesforce 的 Force.com 和 Heroku。

他目前是由 UNIT4 和 Salesforce.com 资助的初创公司 FinancialForce.com 内的首席技术官、Salesforce MVP 和 Salesforce 认证高级开发人员。他负责在产品套件中推动平台采用、关系和技术策略。他是一位热衷于写博客的人、开源贡献者和项目负责人，也是在各种 Salesforce 和 FinancialForce.com 活动上经验丰富的演讲者。

他喜欢看电影、观看 Formula 1 赛车运动，并构建云控制的乐高机器人！你可以在 Twitter 上找到他，账号为 @andyinthecloud，他的乐高机器人 Twitter 账号和网站是 @brickinthecloud。

你可以访问他的 LinkedIn 个人资料 https://www.linkedin.com/in/andyfawcett 和他的网站 https://andyinthecloud.com/。
