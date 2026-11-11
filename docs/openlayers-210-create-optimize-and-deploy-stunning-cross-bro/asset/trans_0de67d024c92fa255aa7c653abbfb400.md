
![](img/1b8cd151c9f278555f9cf255b20d244e_0_0.png)

通过实践学习：少理论，多成果

## OpenLayers 2.10

使用 OpenLayers JavaScript 网络地图库创建、优化和部署精美的跨浏览器网络地图

## 初学者指南

Erik Hazzard

![](img/1b8cd151c9f278555f9cf255b20d244e_0_1.png)

## OpenLayers 2.10

## 初学者指南

使用 OpenLayers JavaScript 网络地图库创建、优化和部署精美的跨浏览器网络地图

Erik Hazzard

![](img/1b8cd151c9f278555f9cf255b20d244e_1_0.png)

BIRMINGHAM - MUMBAI

## OpenLayers 2.10
初学者指南

版权所有 © 2011 Packt Publishing

保留所有权利。未经出版商事先书面许可，本书的任何部分不得以任何形式或任何手段复制、存储在检索系统中或传播，但简短引文用于评论或评论除外。

在编写本书时已尽最大努力确保所提供信息的准确性。然而，本书中包含的信息按原样出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成的任何损害承担责任。

Packt Publishing 已尽力提供有关本书中提及的所有公司和产品的商标信息，适当使用大写字母。然而，Packt Publishing 不能保证此信息的准确性。

首次出版：2011 年 3 月

生产参考：1110311

由 Packt Publishing Ltd. 出版。
32 Lincoln Road
Olton
Birmingham, B27 6PA, UK.

ISBN 978-1-849514-12-5

www.packtpub.com

封面图片由 Jose Argudo (josemanises@gmail.com) 提供

## 致谢

**作者**
Erik Hazzard

**编辑团队负责人**
Aanchal Kumar

**审阅者**
Xurxo Méndez Pérez
Alan Palazzolo
Ian Turton
Couzic Mikael

**项目团队负责人**
Priya Mukherji

**项目协调员**
Jovita Pinto

**收购编辑**
Usha Iyer

**校对**
Steve Maguire

**开发编辑**
Maitreya Bhakal

**图形**
Nilesh Mohite

**技术编辑**
Pallavi Kachare

**生产协调员**
Adline Swetha Jesuthas

**索引员**
Hemangini Bari
Rekha Nair

**封面工作**
Adline Swetha Jesuthas

## 关于作者

**Erik Hazzard** 是一名网页开发者-设计师、开源倡导者和 VI 用户。他喜欢学习、教学，并偶尔在他的网站 http://vasir.net/ 上写博客。作为一名有五年专业网页开发经验的开发者，Erik 专注于 Python 和 JavaScript，尽可能使用开源软件。当他不开发网络应用程序时，他经常在开发或设计电子游戏。

他在 FREAC（佛罗里达资源与环境分析中心）工作，那是一个拥有优秀团队、从事各种 GIS 和网络开发工作的绝佳地方。

我想感谢 OpenLayers 的开发者们，他们不断出色地开发着最好的网络地图框架。我还要感谢我的朋友和导师 Ian Johnson 和 David Arthur，他们给予了我进入网络开发所需的信心和支持。我要感谢 Georgianna Strode 和 Stephen Hodge 给予我的指导、建议以及让我有机会成为更好的网络开发者的机会。如果没有 Packt 的优秀团队的帮助，我不可能写出这本书；我希望每位作者都能像我一样幸运，拥有这样一支优秀的人才团队。我要感谢我的父母给予我永无止境的支持。最后，我要感谢我的爱人 Alisen，她的理解以及花时间帮助我确保本书尽可能易于阅读。

## 关于审阅者

**Xurxo Méndez Pérez** 出生于 1983 年，出生地是西班牙加利西亚南部的小镇 Ourense。他在那里生活到开始在 A Coruña 大学攻读信息技术学位，该学位于 2008 年完成。

在过去的两年里，他在 A Coruña 大学的计算机架构组工作，开发 GIS 应用程序（大量使用许多 OGC 标准），例如 Sitegal 和 SIUXFor（基于网络 GIS 的应用程序，用于管理加利西亚地区的土地产权并促进其良好利用）、MeteoSIX（一个 GIS 系统，提供加利西亚地区地理定位的观测和预报气象数据访问）等。

他还拥有 3 年以上移动应用程序开发经验，最初使用 JavaME，如今他专注于 Google Android，开发了十多款应用程序，其中一些结合了 GIS 和地理定位、实时响应和多用户需求等概念。

**Alan Palazzolo** 在过去五年里一直在构建大大小小的网络应用程序，其中大部分使用开源内容管理系统 Drupal，并在过程中积累了一些数据可视化和地图方面的经验。他是软件和生活方式中开源方法论的坚定信徒和倡导者。他参与了在双城地区启动 Free Geek 分会，并不断尝试使用技术，特别是互联网，来改善那些比大多数人更不幸的人的生活。

**Ian Turton** 是宾夕法尼亚州立大学的地理学研究员。他在近 20 年前偶然成为地理学家，到现在还没有逃脱。在此期间，他是 GeoTools 开源 Java 工具包的联合创始人，该工具包现已成为许多地理开源项目的基础。他继续担任该项目的项目指导委员会成员，并提交新代码和补丁。他还在宾夕法尼亚州立大学和利兹大学教授非常受欢迎的课程“开放网络地图”，使用开放标准和开源程序。

## www.PacktPub.com

## 支持文件、电子书、折扣优惠等

您可能想访问 www.PacktPub.com 获取与本书相关的支持文件和下载。

您知道 Packt 为出版的每本书都提供电子书版本，包含 PDF 和 ePub 文件吗？

您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书籍客户，您有权享受电子书的折扣。请联系我们 service@packtpub.com 了解更多详情。

在 www.PacktPub.com，您还可以阅读一系列免费技术文章，订阅多种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/1b8cd151c9f278555f9cf255b20d244e_6_0.png)

http://PacktLib.PacktPub.com

您需要即时解决 IT 问题吗？PacktLib 是 Packt 的在线数字图书库。在这里，您可以访问、阅读和搜索 Packt 整个图书库。

## 为什么订阅？

- ♦ 可在 Packt 出版的每本书中完全搜索
- ♦ 复制粘贴、打印和书签内容
- ♦ 按需提供并可通过网络浏览器访问

## Packt 账户持有者免费访问

如果您在 www.PacktPub.com 有 Packt 账户，您可以使用它立即访问 PacktLib 并查看九本完全免费的书籍。只需使用您的登录凭据即可立即访问。

## 目录

- 前言 1
- 第 1 章：开始使用 OpenLayers 7
-   OpenLayers 是什么？ 8
-     为什么使用 OpenLayers？ 8
-   从技术上讲，OpenLayers 是什么？ 8
-     客户端 8
-     库 9
-   网络地图应用程序的解剖结构 9
-     网络地图客户端 10
-     网络地图服务器 10
-   与 Google / Yahoo! / 及其他地图 API 的关系 11
-     OpenLayers 中的图层 11
-     什么是图层？ 12
-   OpenLayers 网站 12
-   动手时间 – 下载 OpenLayers 13
-     制作我们的第一张地图 15
-   动手时间 – 创建您的第一张地图 15
-   代码如何工作 17
-     理解代码——逐行 18
-     JavaScript 对象表示法 21
-   背后——面向对象编程（OOP） 24
-     交互发生在对象之间 25
-     MadLibs 25
-   动手时间 – 玩 MadLibs 25
-     使用 OOP 编程 26
-     子类 26
-   现在怎么办？ 27
-     API 文档 28
