
## 博客服务器高级功能

为了生成博客的提要，我们可以轻松遍历帖子，因为我们已经有了一个 ID 列表。为了生成有效的 XML 提要，将使用 `xml` 包。该包不属于 SDK，而是一个开源库，它已成为 Dart 中用于 XML 创建和处理的热门选择。SDK 曾经包含一个 XML 处理包，但 `xml` 包改进得如此之快，以至于将其作为标准包更有意义。让我们看看下面的代码片段以了解更多信息：

```dart
String getRSSFeed() {
  var RssXb = new XmlBuilder();
  RssXb.processing('xml', version: '1.0');

  RssXb.element('rss', attributes: {'version': '1.0'}, nest: () {
    RssXb.element('channel', nest: () {
      IDs.forEach((int postID) {
        BlogPost post = getBlogPost(postID);
        RssXb.element('item', nest: () {
          RssXb.element('pubDate', nest: () {
            RssXb.text(post._date);
          });
          RssXb.element('title', nest: () {
            RssXb.text(post._title);
          });
          RssXb.element('link', nest: () {
            RssXb.text("http://127.0.0.1:8080/post${post._id}.html");
          });
        }); //item
      });
    }); //channel
  }); //rss

  var xml = RssXb.build();
  return xml.toXmlString(pretty: true);
}
```

这里元素以嵌套方式添加，这反映了 RSS 的结构。一旦数据结构组装完成，`build` 方法会构建 XML 结构，最后将其转换为字符串。

对于简单的 XML，直接拼接字符串很有诱惑力。然而，XML 库可以提供诸如字符编码、验证和格式化等功能，这些功能使得稍微冗长的代码在较小的用例中也是值得的。

由于 XML 既会被人类查看，也会被计算机查看，可选命名参数 `pretty` 被设置为 `true`，以输出格式规整的内容，如下例所示：

```
<?xml version="1.0"?>
<rss version="1.0">
    <channel>
        <item>
            <pubDate>02/05/2015</pubDate>
            <title>Lemur Facts</title>
            <link>http://127.0.0.1:8080/post6.html</link>
        </item>
    ...
```

RSS 提要提供给 http://127.0.0.1:8080/feed.xml，并且可以在大多数 Web 浏览器中直接查看。

## 提供 RSS 服务

RSS 提要可以像任何其他文本文件一样提供，但需要设置响应头以帮助客户端应用程序。让我们看看下面的代码片段以了解更多信息：

```
void _serveRSSFeed(HttpRequest request) {
    request.response
        ..statusCode = HttpStatus.OK
        ..headers.set('Content-Type', 'application/rss+xml')
        ..write(hostedBlog.getRSSFeed())
        ..close();
}
```

内容类型设置为 `application/rss+xml`，这会触发浏览器中的 XML 显示。否则，浏览器可能会尝试将内容显示为 HTML。

## JSON 提要的生成

提供 JSON（JavaScript 对象表示法）提要是一种比 XML 更有用的替代方案，因为它需要传输更少的数据，并且通常比处理 XPath 更简单、更快速。`getJSONFeed` 方法是 `Blog` 类的一部分，如下所示：

```
String getJSONFeed() {
    List posts = new List();
    IDs.forEach((int postID) {
        BlogPost post = getBlogPost(postID);
        Map jsonPost = {};
        jsonPost["id"] = post._id;
        jsonPost["date"] = post._date;
        jsonPost["title"] = post._title;
        jsonPost["url"] = "http://127.0.0.1:8080/post${post._id}.html";
        posts.add(jsonPost);
    });
    return JSON.encode(posts);
}
```

此方法与 RSS 采取相同的方法，只是这次主要数据结构是一个列表，条目是该列表中的元素。可以使用 `dart:convert` 中的 `JSON.encode` 方法将列表转换为 JSON：

```
[
  ...
  {"id" : 6,
    "date" : "02/05/2015",
    "title" : "Lemur Facts",
    "url" : "http://127.0.0.1:8080/post6.html"},
  ...
]
```

> JSON 提要提供给 http://127.0.0.1:8080/feed.json，并且可以在大多数 Web 浏览器中直接查看。

## 提供 JSON 服务

提供 JSON 服务不仅需要指定正确的类型，还需要处理从另一个 Web 域消费此数据提要时的安全问题：

```
void _serveJsonFile(HttpRequest request) {
  request.response
    ..statusCode = HttpStatus.OK
    ..headers.set('Content-Type', 'application/json')
    ..headers.add("Access-Control-Allow-Origin", "*")
    ..headers.add(
      "Access-Control-Allow-Methods",
      "POST,GET,DELETE,PUT,OPTIONS")
    ..write(hostedBlog.getJSONFeed())
    ..close();
}
```

内容类型设置为 `application/json`，这会触发浏览器中的 JSON 显示。访问控制（CORS）被设置为允许任何来源以及一组 HTTP 动词。

> 跨源资源共享（CORS）是一种允许从其他 Web 域请求受限资源（如脚本）的技术。例如，新闻网站希望以 JSON 格式请求博客的头条新闻以进行显示。
> 默认情况下，资源只能从同一域加载。CORS 允许共享特定资源以及这些资源上的操作（HTTP 动词），如 GET、POST 等。因此，我们可能乐意共享头条新闻，但不乐意共享登录验证脚本。

## 消费 JSON 提要

JSON 无疑已经成为现代 Web 数据格式，无论是生成还是消费都非常轻量。为了全面探索 JSON 功能，我们将暂时从服务器端转到客户端 Dart，以使用博客的 JSON 数据提要。为此，请查看以下代码片段：

打开本章的示例项目 `BlogJsonClient`。这是一个简单的 Web 项目，充当博客服务器 JSON 提要的客户端。要让此客户端运行，博客服务器也必须正在运行。

```
void main() {
  var jsonSrc = "http://127.0.0.1:8080/feed.json";
  HttpRequest.getString(jsonSrc).then((String data) {
    List decoded = JSON.decode(data);
    decoded.forEach((post) {
      querySelector('#output').children.add(new LIElement()
        ..append(new AnchorElement()
          ..href = post['url']
          ..text = "${post['date']} ${post['title']}");
    }); //for
  }); //then
}
```

这里，`HttpRequest` 类有一个静态方法，可以将远程资源作为 `Future<String>` 返回，并在 `then` 子句中异步处理。

然后对 JSON 进行迭代，并将 `LI` 元素（列表项）添加到网页的 DOM 结构中。

## 静态生成

Web 托管设施因提供商而异。运行程序和安装软件的能力并非普遍可用。随着在云上运行小型简单虚拟机的普及，网站的静态生成变得越来越流行。

其优势非常明显：无需维护软件或数据库，使系统非常安全，同时服务速度也非常快。不过，它并不适用于所有应用。博客是静态生成可能合理的场景之一。

## 冻结网站

在本章的项目中，`bin` 文件夹里有一个 `staticgen.dart` 文件。这是一个独立程序，用于生成博客首页的静态 HTML 文件。请注意，`main` 函数用 `async` 关键字标记，并且缺少 `void` 关键字。

## 介绍 await 和 async 关键字

`await` 和 `async` 关键字是在 Dart 1.9.4 中引入的，目的是简化异步操作。考虑以下对返回 `Future` 的方法的调用：

```
obj.method().then(handlerFunction)
```

这在大多数情况下没问题，但如果事情变得更复杂，并且 `handlerFunction` 也返回一个 future 呢？

```
obj.method().then(handlerFunction).then(handlerFunction2);
```

事情已经开始变得复杂——调试不再简单直观。理想情况下，我们希望一次只处理链条的一部分，并在所需操作完成之前暂停语句的执行。这就是 `await` 所允许的：

```
var f1 = await obj.aMethod();
var result = await f1.aMethod();
```

使用 `await` 调用的函数和方法返回 `Future`。它们还必须在方法头中声明为 `async`，使用 `await` 调用的函数也是如此：

```
class Foo{
  Future<int> aMethod() async {
    return await aFunction();
  }
}
```

