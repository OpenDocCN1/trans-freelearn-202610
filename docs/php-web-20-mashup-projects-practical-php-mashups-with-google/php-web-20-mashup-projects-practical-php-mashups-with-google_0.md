# PHP Web 2.0 Mashup 项目：Google Maps, Flickr, Amazon, YouTube, MSN Search, Yahoo! 实用 PHP Mashup：在 PHP 中创建实用 mashup 获取 ... MSN Search, Yahoo!, Last.fm 和 411Sync




## 致谢

**作者**
Shu-Wai Chow

**项目经理**
Patricia Weir

**审阅者**
Stoyan Stefanov

**项目协调员**
Abhijeet Deobhakta

**高级收购编辑**
Douglas Paterson

**索引员**
Bhushan Pangaonkar

**开发编辑**
Nikhil Bangera

**校对员**
Cathy Cumberlidge

**技术编辑**
Adil Rizwan
Ajay. S

**制作协调员**
Shantanu Zagade

**编辑经理**
Dipali Chittar

**封面设计师**
Shantanu Zagade

## 关于作者

**Shu-Wai Chow** 在过去八年里从事计算机编程和信息技术工作。他在加利福尼亚州萨克拉门托开启职业生涯，在 First Union 公司 Educaid 担任了四年网络管理员，又在 Vision Service Plan 担任了四年应用开发人员。多年来，他已精通 Java、JSP、PHP、ColdFusion、ASP、LDAP、XSLT 和 XSL-FO。Shu 还曾担任萨克拉门托多个动物福利组织的志愿网络管理员和猫类领养顾问。

他目前是新泽西州泽西城 Antenna Software 的软件工程师，并正在新泽西州立大学罗格斯大学攻读经济学学位。

Shu 出生于英属香港殖民地，在加利福尼亚州帕洛阿尔托度过了大部分所谓的成长时光。他与七只非常挑剔的猫、四只聪明过头的鸟、一只无尾鬃狮蜥、一只需要清理鱼缸的斗鱼、一群皮蠹甲虫、一把珍爱的 Fender Stratocaster 以及一位深受爱戴、圣人般的未婚妻一起生活在新泽西海岸。

我在本书中得到了许多不同人士和公司的帮助。

首先要感谢 Packt Publishing 的各位，尤其是 Doug Paterson、Nikhil Bangera、Adil Rizwan 和 Abhijeet Deobhakta，感谢他们专业的合作经历，给了我这个机会，再次感谢他们对我的信任（我仍然完全不明白）。

感谢 Stoyan Stefanov 提供的精彩审阅意见。你的见解和建议真正改善了本书的内容，也在个人层面推动了我更进一步。

每一章都值得特别关注。

第 2 章：感谢 UPC Internet Database 和 Amazon, Inc. 的各位允许使用。特别感谢 UPC Internet Database 的每个人回答我的问题。

第 3 章：感谢 Yahoo! 和 Microsoft 的许可，以及他们法律部门的迅速服务和**协助**。特别感谢 Data Access Corporation 和 Vincent Oorsprong 提供的实用且富有教育意义的 SOAP 服务。Data Access Worldwide (`www.dataaccess.com`) 提供先进的产品和服务，帮助客户构建出色的应用程序并从数据中获得更多价值。

第 4 章：感谢 YouTube 和 Last.fm 的各位允许使用。特别感谢 Last.fm 对我项目的兴趣。

第 5 章：感谢 411Sync 和加州公路巡警的协助。特别感谢 **Manish Lachwani of** 411Sync，他可能是世界上最有耐心的人。

第 6 章：感谢 Google、Flickr 和 Jo Walsh 允许收录。非常感谢 Jo Walsh 的帮助和见解。特别感谢 Google 的各位，他们的热情让我觉得这本书并不是一个疯狂的想法。

我在上一本书中遗漏了对 Ed Hansen 和 Keith Easterbrook 的感谢。他们是迫使我们使用 *WebSphere Application Developer* 的人。没有 WSAD，我永远不会使用 Eclipse，也永远不会使用 PHPEclipse，这意味着我永远不会写第一本书，而没有第一本书，就不会有这第二本书。所以，感谢两位先生，我为疏忽道歉。

感谢新泽西州贝尔马的 Kaya’s Kitchen 制作了全世界最好的素食美食。如果你有机会来到新泽西海岸，你绝对必须光顾这家餐厅。

最后，致敬帕洛阿尔托鸭塘、El Camino 和 Arastradero 的 Hobee’s、道奇体育场以及戴维斯绿带。

## 关于审阅者

**Stoyan Stefanov** 是 Yahoo! 的网页开发人员、Zend 认证工程师、图书作者以及国际 PHP 社区的贡献者。他的个人博客位于 http://www.phpied.com。

很久以前，一位占卜者告诉我，我会在二月份在地球上遇到一位天使。本书献给那位二月份的天使和我生命的挚爱 Anneliese Strunk。
你为我的生活带来的幸福、灵感和喜悦是我从未想象过的。

## 前言

Mashup 是将来自两个或多个外部在线源的数据组合成集成体验的网页或应用程序。本书是您进入 Mashup 和 Web 2.0 世界的入口。您将创建 PHP 项目，从 Web 上的一个地方抓取数据，将其与来自 Web 上另一个地方的相关信息混合，并在单个应用程序中呈现。书中使用的所有 Mashup 应用程序都建立在免费工具之上，并得到了充分解释。您可以在我们网站的代码下载部分找到构建 Mashup 所使用的所有源代码。

本书是一本实用教程，包含五个详细且解释清晰的案例研究，用于构建新的有效 Mashup 应用程序。

## 本书涵盖内容

您将学习如何编写 PHP 代码以远程使用 Google Maps、Flickr、Amazon、YouTube、MSN Search、Yahoo!、Last.fm 和 Internet UPC 数据库等服务，更不用说加州公路巡警交通数据了！您还将了解使用这些 Web 服务和 API 所需的技术、数据格式和协议，以及一些可自由使用的 PHP 工具。

您将了解这些技术如何协同工作，并了解如何将此信息与您的想象力结合起来构建自己的前沿网站。

第1章提供了 Mashup 的概述：Mashup 是什么，以及您为什么想要一个。

在第2章中，我们创建了一个基本的 Mashup，并进行购物。我们将根据通用产品代码 (UPC) 在 Amazon.com 上查找产品。为此，我们将介绍两种基础 Web 服务以入门——XML-RPC 和 REST。Internet UPC 数据库是基于 XML-RPC 的服务，而 Amazon 使用 REST。

我们将创建代码来调用 XML-RPC 和 REST 服务。使用 PHP 的 SAX 函数，我们创建了一个可扩展的面向对象 XML 解析器。本章介绍的 Mashup 将来自 Amazon 电子商务服务 (ECS) 的信息与 Internet UPC 数据库集成。

在*第3章*中，我们使用 MSN 和 Yahoo! 的技术创建自定义搜索引擎。本章从介绍 SOAP 开始，SOAP 是最复杂的 Web 服务协议之一。SOAP 严重依赖于 WSDL 和 XSD 等其他标准，这些标准也在书中以易读的细节进行了介绍。我们查看 WSDL 文档并学习如何确定其中可用的 Web 服务以及传递的数据类型。使用 PHP 5 的 SoapClient 扩展，我们随后与 SOAP 服务器交互以获取数据。然后我们最终创建了我们的 Mashup，它收集来自 Microsoft Live 和 Yahoo! 的网络搜索结果！

对于*第4章*中的 Mashup，我们使用视频存储网站 YouTube 的 API，以及社交音乐网站 Last.fm 的 XML Feed。我们将查看这两个网站的三种不同的基于 XML 的文件格式：用于歌曲播放列表的 XSPF、用于发布频繁更新信息的 RSS，以及 YouTube 的自定义 XML 格式。我们将创建一个 Mashup，它获取两个 Last.fm RSS Feed 中的歌曲并查询 YouTube 以检索这些歌曲的视频。 rather than creating our own XML-based parsers to parse the three formats, we have used parsers from PEAR, one for each of the three formats. Using these PEAR packages, we create an object-oriented abstraction of these formats, which can be consumed by our mashup application.

在*第5章*中，我们从加州公路巡警网站进行屏幕抓取。CHP 维护着一个交通事件网站。该网站每分钟自动刷新，确保用户获得加州各地事故的实时数据。如果您坐在电脑前，这非常有价值。如果您外出办事，这就相当没用了。然而，我们的 Mashup 将使用 411Sync.com 的 Web 服务来接受移动用户的 SMS 消息，以便将这些交通事件传递给用户。

我们几乎把所有东西都放进了*第6章*！在本章中，我们使用 RDF 文档、SPARQL、RAP、Google Maps、Flickr、AJAX 和 JSON。我们创建了一种以地理为中心的方式在 Google Maps 上展示 Flickr 照片。我们了解如何读取 RDF 文档以及如何使用 SPARQL 和用于 RDF 的 RAP 从中提取数据。这为我们提供了伦敦地铁站的纬度和经度。我们在 Google 地图上显示它们，并从 Flickr 检索所选车站的照片。我们的应用程序需要与 API 服务器通信，为此我们使用 AJAX 和 JSON，而 JSON 正成为一种主要的数据格式。此 AJAX 应用程序最大的陷阱是竞态条件，我们将学习各种克服这些问题的方法。

## 您需要的内容

要跟随项目并使用本书中的示例代码，您需要运行 PHP 5.0 或更高版本和 Apache 1.3 的 Web 服务器。

所有示例都假设您在本地工作站上运行 Web 服务器，并且所有开发都在本地进行。

此外，两个项目有特殊要求。在第5章中，您需要访问可以从 Internet 外部访问的 Web 服务器。在第6章中，您需要一个 MySQL 服务器。同样，我们假设您在本地运行 MySQL 服务器并且它已正确配置。

要快速安装 PHP、Apache 和 MySQL，请查看 XAMPP (http://www.apachefriends.org/en/xampp.html)。XAMPP 是 PHP、Apache 和 MySQL 等的一步安装程序。

XAMPP 可用于 Windows、Linux 和 Mac OS X。然而，许多标准 Linux 发行版已预装了 PHP、Apache 和 MySQL。请查阅您发行版的文档，了解如何激活它们。Mac OS X 默认已安装 Apache 和 PHP。您可以通过在共享首选项中启用 Web 共享来打开它们。

MySQL 可以作为从 MySQL.com (http://dev.mysql.com/downloads/mysql/4.1.html) 下载的二进制文件进行安装。

## 约定

本书中，您会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义的解释。

代码有三种样式。文本中的代码词显示如下："We can include other contexts through the use of the include directive."

代码块将如下设置：

```
<?php
    $aDom = new DOMDocument();
    try {
        $aDom->loadHTMLFile('examplehtml.html');
    } catch (Exception $ex) {
        $aDom = false;
    }
}
```



当我们希望引起您对代码块中特定部分的注意时，相关行或项目将被加粗：

```
<param>
    <value><string>Hello, world!</string></value>
</param>
```

任何命令行输入和输出都写成如下形式：

```
Buttercup:~ root# pear list
```

Buttercup:~ root# 是作者机器上的 shell 提示符。

**新术语**和**重要词语**以粗体字体引入。您在屏幕上看到的词语，例如在菜单或对话框中，会在我们的文本中这样出现："在搜索框中，输入您的关键词和区域代码然后按 **搜索**。"

重要提示会以如下的框形式出现。

技巧和窍门会以如下形式出现。

## 读者反馈

来自我们读者的反馈总是受欢迎的。请告诉我们您对这本书的看法，您喜欢或可能不喜欢的部分。读者的反馈对我们开发您真正能充分利用的书名非常重要。

如需发送一般反馈，只需发送电子邮件至 `feedback@packtpub.com`，并确保在邮件主题中提及书名。

如果您需要一本书并且希望我们出版，请在 `www.packtpub.com` 上的 **建议书名** 表单中发送给我们留言，或发送电子邮件至 `suggest@packtpub.com`。

如果您在某个主题方面有专业知识，并且有兴趣撰写或为一本书做出贡献，请参阅我们位于 `www.packtpub.com/authors` 的作者指南。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项帮助以助您充分利用购买。

### 下载本书的示例代码

访问 http://www.packtpub.com/support，并从标题列表中选择本书以下载本书的任何示例代码或额外资源。随后可下载的文件将被显示。

可下载文件包含如何使用它们的使用说明。

### 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。通过这样做，您可以让其他读者免于沮丧，并帮助改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的图书，点击提交勘误链接，并输入勘误的详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将被添加到现有勘误列表中。现有勘误可以通过从 http://www.packtpub.com/support 选择您的标题来查看。

### 问题

如果您在书的某些方面遇到问题，可以联系我们 questions@packtpub.com，我们将尽力解决。
