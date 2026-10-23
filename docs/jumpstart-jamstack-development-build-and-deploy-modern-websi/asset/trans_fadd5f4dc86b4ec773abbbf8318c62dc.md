

# 快速启动 Jamstack 开发

使用 Gatsby、Netlify 和 Sanity 构建和部署现代网站和 Web 应用

![](img/97ab031f38e2174e2f1b51da18dfa1d0_0_0.png)

Christopher Pecoraro | Vincenzo Gambino

Sanity 首席执行官 Magnus Kongslø Hillestad 序

# 序

创新往往由那些期望更多并挑战现状的人推动。他们不是对现状做出小幅改进，而是开辟新的道路，这些道路随着时间的推移变得不可避免。我们最终会达到一个无法想象回到过去的程度。

今天构建网页最常见的解决方案诞生于 2000 年代初期。曾经的尖端技术如今限制了数字项目和内容协作，让开发者和内容编辑感到沮丧，并让产品和业务所有者陷入一种基础设施困境，这种基础设施会造成糟糕的数字客户体验并损害其数字化转型的能力。

我们现在终于看到了重新思考我们在网络上处理内容和数字体验方式的趋势，而通常被称为 Jamstack 的框架和服务是这场革命的重要组成部分。它们是对传统单体 Web 架构限制的回应：组织需要更多安全性、可扩展性，并以不断增长的交付速度满足受众对更好内容的需求。

Jamstack 以单体系统无法实现的方式满足了这些需求。这些工具正在重新定义网络上可能实现的事情——而这本书正是你迈入这种新构建方式所需的入门指南。Christopher Pecoraro 和 Vincenzo Gambino 提供了逐步流程，展示如何结合三种创新且领先行业的工具，构建你自己的超快网站和实时后端：

- **GatsbyJS**：广受欢迎的基于 React 的静态站点生成器
- **Netlify**：领先行业的 Jamstack Web 主机和内容分发网络
- **Sanity.io**：实时内容平台，让你将内容视为数据

除了制作快速网站之外，本书还展示了这些工具如何用于向任何应用程序或设备呈现内容，其典型例子是从驱动你网站的相同内容中驱动 Amazon Alexa 技能。

这正是本书介绍给你的工具的真正承诺——它们的能力远不止网站。这种由 API 驱动的微服务、结构化内容和无服务器计算的星座也支持更高的内容速度目标。有远见的团队正在拥抱这样一个事实：优秀的网站只是运行统一内容项目的第一步。他们正在引领一种新的工作方式——产品和营销内容及工作流程围绕单一事实来源汇聚，而交付令人惊叹的全渠道体验不再是梦想。

既然你正在阅读这本书，你可能是一个期望更多、挑战现状并探索前沿的人。我们一起正在创造范式转变，不仅在于网络如何构建，也在于我们处理内容以及构建数字体验的整体方法。

在另一边见……

*Magnus Kongsli Hillestad*

*Sanity.io 首席执行官*

# 版权

Copyright © 2021 Packt Publishing

保留所有权利。未经出版商事先书面许可，本书的任何部分不得以任何形式或任何手段复制、存储在检索系统中或传播，但嵌入在评论或评论文章中的简短引文除外。

在准备本书时已尽最大努力确保所提供信息的准确性。然而，本书所包含的信息按“现状”出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已尽力提供本书中提及的所有公司和产品的商标信息，并以适当的大写形式使用。然而，Packt Publishing 不能保证此信息的准确性。

**产品集团经理：** Ashwin Nair
**出版产品经理：** Ashitosh Gupta
**高级编辑：** Sofi Rogers
**内容开发编辑：** Abhishek Jadhav
**技术编辑：** Shubham Sharma
**校对编辑：** Safis Editing
**项目协调员：** Manthan Patel
**校对：** Safis Editing
**索引：** Priyanka Dhadke
**制作设计师：** Roshan Kawale

**首次出版：** 2021 年 5 月
**生产参考：** 1070521

由 Packt Publishing Ltd 出版。
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80020-349-5

www.packt.com

# 致谢

本书谨以此纪念并深切怀念我的父亲

Dr. George Anthony Pecoraro, Sr.

他为我们付出了一切

也纪念我的朋友

Nicholas Robert Pinansky עבשילא'י דוד רב לאומנע

以及我的亲爱的表亲们

Sergeant Gregory Lee Smith

和

Airman First Class Victor Joseph Guida

Questo libro è stato scritto in memoria del Professor Ingegnere Serafino Casu.

È stato per me come un padre.

我特别要感谢我的妻子，Dr. Anna Casu, sa prenda mea, 她拥有神圣的耐心、伴侣之爱和永恒的支持，以及我的母亲 Patricia Marie Pecoraro，她无私且难以形容的爱。感谢我在美国和意大利的家人。感谢我的教育者 Br. David Carlson OSB、Dr. Colleen Carr 和 Saint Vincent College 的 Dr. Harold Morrison。

我要感谢 Luke Woodling、Cliff Manspeaker、Clinton J Robinson、Logo Olagoke 以及我在 Rollins College 的所有其他同事。

此外，特别感谢 Aaron LeClair 相信我，以及 Harmony Betancourt、Efrain Lugo 和 Nick Clements 持续的鼓励和友谊。

致我的前同事：Geoffrey Bent、Steven Dudukovich、Nate See、Shen DeShayne，以及许多我不幸忘记的人。

感谢 Sanity：感谢 Magnus 喜欢我的推文，这激发了本书的灵感，以及 Knut、Kapehe 和 Bryan 在写作过程中的持续支持。

最后但绝非最不重要的，GRAZIE 致我的合著者 Vincenzo，以及 Silvia 和 Thea。

*我们深切缅怀在 COVID-19 大流行中失去生命的人们，并向为战胜它付出一切的人致敬。*

— Christopher Pecoraro

献给我的挚爱妻子 Silvia，她的力量和决心激励着我。你是我永远的完美妻子，也是我们孩子的母亲。我真的爱你，我是认真的。

献给我“睡在蒲公英上”的女儿 Thea Sofia：选择你自己的土地，追随你的梦想。

献给我母亲 Anna Maria，我父亲 Giuseppe 和我的姐姐 Alice，以爱和感激。

献给我侄女 Emma 和侄子 Massimiliano、Alessandro P.、Gabriele 和 Alessandro S.，他们总是让我微笑。

纪念我的祖父母 Rocco Battaglia、Angela Cardinale、Vincenzo Gambino 和 Rosalia Bronzino。

献给我合著者和朋友 Christopher Pecoraro，他支持我在编程事业上的发展。

- Vincenzo Gambino

# 目录

## 前言

## 1
### Jamstack 的历史
Jamstack 的演变 2
HTML 2
服务器端包含 3
通用网关接口 3
表单 4
网页预处理器 4
内容管理系统 4
WordPress 5
Ajax 5
JavaScript 框架 5
云的兴起 5
高速访问 6
Jamstack 的兴起 6
Jamstack 缩写解释 7
Jamstack 优势 7

### 我们的 Jamstack 9
GatsbyJS 9
Sanity.io 9
Netlify 9

### 开始使用 Jamstack 9
安装 10
先决条件 10

### 总结 11

## 2
### Sanity 简介
技术要求 14
账户设置 14
Sanity.io 简介 14
什么是 GROQ？ 15
创建项目 15

### 介绍 Sanity Manage 19
Sanity.io 项目页面 19

### Sanity Studio 概览 22
导航 22

### 总结 25

