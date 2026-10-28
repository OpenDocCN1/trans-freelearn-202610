

## 简介

当你第一次编译这个项目时，它看起来像这样：

![](img/4e826fa36038b86f2087ef8b818d0acf_30_0.png)

没什么可看的。但用不了多久，你就会添加各种功能，让它能够用来浏览 Facebook，渲染不同的页面和照片：

![](img/4e826fa36038b86f2087ef8b818d0acf_30_1.png)

## 简介

然而，本书的主要目的是教你 Facebook Flash 开发背后的底层概念；一旦你理解了这些，实际使用的代码和 SDK 就无关紧要了。因此，本书还会教你如何从头开始编写你可能需要的每一种 Facebook 交互程序。代码将完全属于你自己，你会理解每一行，没有任何抽象阻碍。

除了用于 Facebook 平台的 Adobe AS3 SDK 之外，还大量使用了另外两个代码库：

- **MinimalComps**：Keith Peters 编写的出色轻量级用户界面组件，可在 http://www.minimalcomps.com/ 获取，采用 MIT 许可证。
- **as3corelib**：一组用于处理 AS3 的类和工具，包括用于 JSON 序列化的类，可在 https://github.com/mikechambers/as3corelib 获取，采用 BSD 许可证。

## 调试

从第 3 章开始，你的 SWF 需要从你的服务器上通过 Web 浏览器运行才能正常工作。（原因请看那一章。）这使调试变得棘手——浏览器中没有“输出”面板，因此 trace 语句不会自动可见。

Visualizer 包含一个对话框功能，你可以用它来解决这个问题。可以从显示列表中的任何类创建它。为此，首先导入 DialogEvent 类：

```
import events.DialogEvent;
```

然后，分发一个类型为 DIALOG 的 DialogEvent，参数中包含你希望输出的文本：

```
dispatchEvent(new DialogEvent(DialogEvent.DIALOG, "Example"));
```

它看起来像这样：

![](img/4e826fa36038b86f2087ef8b818d0acf_32_0.png)

当然，这只对 Visualizer 项目有用。当你构建自己的项目时，又能做些什么呢？

## 简介

在 Google Chrome 中，你可以通过点击[扳手] | 工具 | 清除浏览数据...，选择“清空缓存”，并选择适当的时间段来完成：

![](img/4e826fa36038b86f2087ef8b818d0acf_34_0.png)

![](img/4e826fa36038b86f2087ef8b818d0acf_34_1.png)

你应该可以轻松地通过 Google 搜索«浏览器名称»删除缓存，为你的浏览器找到等效选项。

## 最后说明...

Facebook 的开发人员一直在调整这个平台。这会使得在其上开发令人兴奋，因为新功能总在不断添加；但也可能让人非常沮丧，因为旧功能可能会被移除，或者其实现方式发生变化；任何内容都可能在任何时候被修改。

新的平台 API（Graph API）是一个坚实的基础，而且看起来会持续一段时间——请记住，以前的平台 API 持续了四年。但它是模块化的，各个部分可能会发生变化，甚至可能被移除。

因此，本书的部分内容在你阅读时可能已经过时，某些指令在当前的 Facebook 平台版本上可能不会产生与本书编写时相同的结果。如果你担心这一点，可以在*第 8 章：跟上扎克伯格家族*的最后一节了解如何跟上平台的所有变化。

但现在，请直接进入*第 2 章：欢迎来到图谱*，开始开发吧！

## 2

## 欢迎来到图谱

> Facebook 拥有关于人物、公司、活动、相册等的大量信息。它也知道这一切是如何关联的：哪个人拥有哪个相册；哪些人出现在哪张照片中；哪个公司组织哪场活动。

四年来，这些信息都是通过一个庞大而杂乱的 API 来访问的，随着新功能的不断加入，它变得越来越复杂。2010 年 4 月，Facebook 推出了 Graph API，极大地简化了开发人员检索所有这些数据的方式。

在本章中，我们将：

- 探索 Facebook 图谱
- 了解 Graph API 是什么，以及它如何组织 Facebook 上的所有数据
- 使用 AS3 和 Graph API 访问公开图谱数据

那么让我们开始吧。

## 通过浏览器访问 Graph API

我们直接来看看 Graph API 如何表示一个公共页面的信息。

当我提到大写 P 的页面（Page）时，我指的不是 Facebook 网站中的任意网页；我指的是一种特定类型的页面，也称为公共主页。每个 Facebook 用户都有自己的个人主页；你可以通过登录 Facebook 并点击网站顶部导航栏中的“主页”链接来查看自己的个人主页。公共主页看起来类似，但它是为商家、乐队、产品、组织和公众人物设计的，作为他们在 Facebook 上存在的一种方式。

这意味着许多人同时拥有个人主页和公共主页。例如，Facebook 的首席执行官马克·扎克伯格在 http://www.facebook.com/zuck 有一个个人主页，在 http://www.facebook.com/markzuckerberg 有一个公共主页（页面）。这样，他可以用个人主页与朋友和家人保持联系，同时用公共主页与粉丝和支持者建立连接。

还有第二种页面类型：社区主页。同样，这些页面看起来与个人主页非常相似；区别在于它们基于主题、经历和事业，而不是实体。此外，它们会自动从维基百科检索相关主题的信息，并包含一个实时更新的关于该主题的帖子动态。

这一切可能让人有点困惑——别担心！一旦你开始使用，一切都会变得有意义。

### 动手实践 —— 加载一个页面

浏览到 http://www.facebook.com/PacktPub 加载 Packt Publishing 的 Facebook 页面。你会看到最近的帖子列表、一个信息标签、一些相册（大多包含书籍封面）、一张主页头像照片，以及粉丝和链接的列表。

## Packt Publishing

主页  信息  照片  讨论

添加到我的主页收藏

推荐给朋友

通过短信订阅

我们的书籍和出版物分享您的 IT 同行在采用和定制当今系统、应用程序和框架方面的经验。

# 信息

成立时间：
2004 年

# 412 人赞

# 照片

3 个相册中的 2 个  查看全部

主页照片
约 2 个月前更新。

书籍
约 7 个月前更新。

# 链接

449 个链接中的 3 个  查看全部

Packt 开源畅销书优惠 | Packt Publishing 技术与 IT 书店
10 月 22 日 13:35

Packt 最新发布：PHP 5 社交网络 | Packt Publishing 技术与 IT 书店
10 月 22 日 10:41

Packt 全新十月开源书籍 | Packt Publishing 技术与 IT 书店
10 月 22 日 09:45

# 基本信息

成立时间：2004 年

# 详细信息

网站：http://www.PacktPub.com

公司概述：Packt 是一家现代 IT 图书出版商，专注于为开发人员、管理员和新手社区出版前沿书籍。

Packt 于 2004 年 4 月出版了第一本书《Mastering phpMyAdmin for MySQL Management》。

Facebook 页面：http://www.facebook.com/PacktPub

这就是网站用户查看信息的方式。我们的代码会如何“看待”它？让我们看一下 Graph API 如何表示 Packt Publishing 的页面：在浏览器中访问 https://graph.facebook.com/PacktPub。这称为 Graph URL——请注意，它与页面本身的 URL 相同，但使用的是安全的 https 连接，并使用 graph 子域，而不是 www。

你将看到如下内容：

```
{
  "id": "204603129458",
  "name": "Packt Publishing",
  "picture": "http://profile.ak.fbcdn.net/hprofile-ak-snc4/hs302.ash1/23274_204603129458_7460_s.jpg",
  "link": "http://www.facebook.com/PacktPub",
  "category": "Products_other",
  "username": "PacktPub",
  "company_overview": "Packt is a modern, IT focused book publisher, specializing in producing cutting-edge books for communities of developers, administrators, and newbies alike.\n\nPackt published its first book, Mastering phpMyAdmin for MySQL Management in April 2004.",
  "fan_count": 412
}
```

## 刚才发生了什么？

你刚刚在浏览器中获取了 Graph API 对 Packt Publishing 页面的表示。

Graph API 的设计目标是易于上手——几乎可以说是自文档化的——你可以看到它在这方面非常成功。很明显，上述数据是一个字段及其值的列表。

有一个字段可能不太清楚：id；这个数字是 Facebook 在内部用来引用该页面的。这意味着页面可以有两个 ID：一个是 Facebook 自动分配的数字 ID，另一个是页面所有者选择的字母数字 ID。这两个 ID 是等价的：如果你浏览 https://graph.facebook.com/204603129458，你会看到与浏览 https://graph.facebook.com/PacktPub 完全相同的数据。

## 自己动手 —— 探索其他对象

当然，Packt Publishing 页面并不是你可以在浏览器中使用 Graph API 探索的唯一页面。通过 Facebook 网站找到一些其他页面，然后使用 https://graph.facebook.com/id 格式，查看它们的 Graph API 表示。它们的信息是更多，还是更少？

