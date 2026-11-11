
- 第4章：您自己的视频点唱机 125
  - 项目概述 125
- XSPF 126
- RSS 129
- YouTube 概述 136
  - YouTube 开发者 API 138
- Last.fm 概述 140
  - Audioscrobbler Web 服务 141
- 使用 PEAR 解析 142
  - 包安装和使用 143
  - File_XSPF 144
  - Services_YouTube 147
  - XML_RSS 149
- 混搭 153
  - Mashup 架构 153
  - 主页 154
  - 导航页 154
  - 内容页 156
  - 使用 Mashup 158
- 摘要 161
- 第5章：通过 SMS 获取交通事件 163
  - 项目概述 163
- PHP 方式的屏幕抓取 164
  - 使用 DOM 函数解析 167
  - 基本元素和属性解析 168
  - 测试模式 171
  - 关于 PHP 的 DOM 实现的更多内容 172
- 411Sync.com API 179
  - 创建您的移动搜索关键词 180
    - 为关键词命名 181
    - 用户使用您的搜索时使用的格式 182
    - XML 数据的 HTTP 位置 182
- 加州公路巡警事件页面 183
- 混搭 190
  - Incident 类 191
  - DOM 解析器类 191
  - CHP DOM 解析器类 193
  - 创建 Feed 页面 199
  - 测试和部署 200
- 摘要 201
- 第6章：伦敦地铁照片 203
  - 项目概述 203
  - 初步规划 204
  - 查找地铁信息 205
  - 集成 Google Maps 和 Flickr 服务 206
  - 应用序列 207
  - 资源描述框架 (RDF) 207
  - SPARQL 210
    - 分析查询主题 210
    - SPARQL 查询的结构 211
    - 编写 SPARQL WHERE 子句 213
      - 基本原则 213
      - 一个简单查询 214
      - 查询类型 217
      - 排序、限制和偏移 219
      - UNION 和 DISTINCT 220
    - 更多 SPARQL 功能 221
  - PHP 的 RDF API (RAP) 221
  - XMLHttpRequest 对象 224
    - XMLHttpRequest 对象概述 226
    - 使用对象 226
      - 创建对象 226
      - 发出 HTTP 请求 227
      - 创建和使用回调 228
  - JavaScript 对象表示法 (JSON) 231
    - JavaScript 对象回顾 231
    - JSON 结构 232
    - 访问 JSON 属性 233
    - 序列化 JSON 响应 234
  - Google Maps API 235
    - 创建地图 235
    - 地理编码 236
    - 标记 239
    - 事件 240
    - InfoWindow 框 240
  - Flickr 服务 API 243
    - 执行搜索 244
    - 解释服务结果 245
    - 检索照片或照片页面 247
  - 混搭 249
    - 构建和填充数据库 249
      - 检查文件 249
      - 创建我们的数据库模式 250
- 构建 SPARQL 查询 251
- 车站查询 252
- 线路查询 253
- 线路到车站查询 253
- 数据库填充脚本 254
- TubeSource 数据库接口类 261
- 主用户界面 262
- 使用 AJAX 的 Flickr 服务 267
  - 创建 XMLHttpRequest 代理 267
  - 修改主 JavaScript 269
  - 发出 XMLHttpRequest 269
  - 竞态条件 271
  - 解析 AJAX 响应 273
- 摘要 278
- 索引 279

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
