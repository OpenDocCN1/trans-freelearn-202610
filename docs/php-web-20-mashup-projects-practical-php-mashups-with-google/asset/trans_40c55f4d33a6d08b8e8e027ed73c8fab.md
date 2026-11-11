
![](img/c637937d44400f1da21fb7cca101de80_76_0.png)

在继续之前，我们需要对所处理的“业务逻辑”有所了解。我们需要澄清通用产品代码 UPC 与欧洲商品编号 EAN（又称 European Article Number）之间的区别。UPC 是大多数人熟悉的，尤其是在美国。它是产品条形码下方的十二位数字。EAN-13 是 UPC 的更新版本。它有十三位数字。EAN 已在欧洲使用多年，如今已在全球范围内采用。UPC 开始逐渐消失，但我们在现实世界中仍会遇到它们，并且该术语在文化用语中作为这类数字的通用指代。然而，我们稍后需要记住技术上的区别，当用户提供十二位数字与十三位数字时。

两者都向我们返回一个结构体，xmlrpc_decode 会将其转换为关联数组。我们可以通过使用已知的 EAN 调用服务来查看返回给我们哪些信息。

```php
$parser = new XMLRPCParser();
$returnedData = $parser->callService('lookupEAN',
    '0737628087501',
    'www.upcdatabase.com',
    '/rpc',
    80);
$parser->processXMLRPCResponse($returnedData);
```

我们在 processXMLRPCResponse 中的通用 echo 代码将在浏览器中显示数组内容。

| 数组键 | 示例值 | 描述 |
|---|---|---|
| upc | "737628087501" | 该产品的 UPC 代码。 |
| pendingUpdates | 0 | 数据库中该产品的待更新数量，如有。 |
| isCoupon | false (Boolean) | 如果此 UPC/EAN 为优惠券。 |
| ean | "0737628087501" | 该产品的 EAN。 |
| issuerCountryCode | "us" | UPC/EAN 的国家代码 |
| Description | "Thai Kitchen Noodle Cart Single Serve Size Thai Peanut Instant Rice Noodles & Sauce" | 产品描述。 |
| found | true (Boolean) | 如果条目在数据库中被找到。 |
| size | "2.25 oz (64 g)" | 产品尺寸或重量。 |
| message | "Database entry found" | 来自服务的消息响应。 |
| issuerCountry | "United States" | 该产品来源地。 |
| lastModified | "2006-04-17 21:42:26" | 该产品在数据库中最后修改时间。字符串，不是 ISO8601 日期。 |

如果 UPC/EAN 有效但在数据库中未找到，则只返回其中两项。found 将为 false，且 message 将为 "No database entry found"。我们可以使用其中任意一项来测试结果。

请注意，查询会同时返回 UPC 和 EAN。这是 Internet UPC Database 的一个微妙特性。每次搜索都会翻译 UPC 和 EAN。这种翻译对我们的 mashup 至关重要。当我们更仔细地查看 Amazon Web Services 时，我们会发现如果搜索除 Amazon 美国站之外的任何 Amazon 商店，它可以直接使用 EAN 获取产品。如果搜索 Amazon 美国站，则必须提供 UPC。即使 EAN 已在美国货架上出现，美国站也不支持 EAN 搜索。反之，如果你手头有一个 UPC 编号并想在 Amazon 英国站搜索它，则必须将其转换为 EAN。

Internet UPC Database API 是一个基于简单 Web 服务协议的简单 API。现在让我们来看一个更复杂的 API，Amazon Web Services。

## Amazon API

Amazon Web Services（http://www.amazon.com/aws）是针对各种 Amazon 产品的一整套 Web 服务。Amazon 提供的产品中包括虚拟计算服务、数字存储服务和消息队列服务（你还以为他们只卖 DVD 和书……）。其中一些服务是收费的，如历史价格数据，而另一些服务，如 Alexa Web Service，则是免费的或在一定请求次数内免费。毫不奇怪，Amazon 为其传统的电子商务产品如图书和 DVD 提供了免费的 Web 服务，称为 Amazon 电子商务服务（ECS）。

在架构上，这些 Web 服务可作为 REST 或 SOAP 提供。根据 Amazon 首席 Web 服务布道者 Jeff Barr 的说法，其 85% 的 Web 服务开发者使用其 REST 服务。稍后，我们将为另一个 mashup 查看 SOAP，但现在，我们将加入那 85%，使用更简单的 REST 服务。

我们将要查看的是 ECS。在开始之前，你需要在 Amazon 注册一个 Access Key ID。该 ID 本身是免费的，可通过在 https://aws-portal.amazon.com/gp/aws/developer/registration/index.htm 注册获得。每当你使用 Amazon 的任何 Web 服务时，都需要将此密钥作为 Amazon 请求的参数之一传递。

### A Tour of ECS

Amazon 电子商务服务非常庞大。打印出来，开发者文档超过 450 页。ECS 有一个 Web 服务功能，他们称之为 operations（我们将沿用此术语），涵盖你在真正的 Amazon.com 上能做的所有事情。首先，对于此处涉及的任何内容的更多详细信息，你可以在 http://developer.amazonwebservices.com/connect/kbcategory.jspa?categoryID=5 找到 ECS 文档。那里收集了大量参考资料、操作指南和最佳实践论文。API 文档的当前和以前版本位于 http://developer.amazonwebservices.com/connect/kbcategory.jspa?categoryID=19。在撰写本书时，当前版本日期为 2007 年 8 月 30 日。你完全可以利用 ECS 构建一个 Amazon 克隆。关于 ECS 的整本书都已出版，而且篇幅也很长。对于我们的小型 mashup，我们将只关注几个功能。然而，值得一看使用 ECS 还能做什么：

- **搜索产品**
  ECS 具有强大的搜索功能。产品搜索分为各个索引。这些索引用于产品类别——图书、DVD、电子产品、运动用品等。还有组合索引，可将两个或多个索引组合在一起。例如，“video” 实际上是 DVD 和 VHS 索引的组合。每次搜索都可提供大量搜索参数。制造商、价格范围和状况等参数可让你精细调整搜索。
- **产品的详细信息**
  一旦找到所需产品，你可以检索该产品的大量信息。产品的基本信息，如价格、标题和描述，只是开始。Amazon 产品页面上几乎所有内容都可通过 ECS 返回，例如图片、评论、Listmania、销售排名和配件。专辑还可以返回专辑中的曲目列表。
- **通过 Web 服务的购物车**
  电子商务网站的购物车并不难创建，但可能很繁琐。幸运的是，ECS 有一个完整的部分来创建你自己的 Amazon 购物车。使用 ECS，你可以创建购物车、添加和删除商品，并将其传递到结账。
- **XSL 支持**
  ECS 的 REST 响应为 XML 格式。开发者需要正确处理和展示数据。XSLT 的支持使这一过程更简单。你可以提供的一个参数是 XSLT 模板的 URL。ECS 会在返回给你之前自动将 XSLT 模板应用于 XML 结果。
- **检索客户信息**
  ECS 有一种特殊模式可按姓名或电子邮件地址搜索客户。不能返回任何私人信息，如地址或信用卡信息。然而，你可以获取任何公开信息，如评论和愿望清单。
- **检索卖家信息**
  相反，你可以获取 Amazon Marketplace 卖家或商户的信息。同样，私人信息不可用，但你可以检索你在 Amazon.com 上看到的任何信息。你还可以检索他们当前销售的商品列表。
- **餐厅信息**
  坦白说，这似乎不符合 Amazon 的传统产品。然而，你可以通过 ECS 查询餐厅信息。城市数量有限，但你可以按价格范围、社区和菜系搜索。

### Anatomy of an ECS REST Request

了解如何发出 ECS 请求需要知道两件事。第一是服务的地址。第二是每个请求中的参数。每个请求都包含一些基本全局参数以及特定于操作的。

我们将在本节中查看所需的全局参数。很快，当我们开始 mashup 时，我们将看到一些操作特定参数的实际应用，当我们使用操作并在 ECS 文档中查找它们时。

### Location of Service

我们希望调用的服务的 URL 完全取决于我们想要使用的 Amazon 位置。
