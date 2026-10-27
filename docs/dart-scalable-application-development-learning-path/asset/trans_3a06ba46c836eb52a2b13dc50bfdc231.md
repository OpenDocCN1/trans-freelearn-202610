

## 启动服务器

博客服务器的 main.dart 入口点与 Hello World 示例类似。提供内容的实际功能由 BlogServerApp 类处理，其中初始化了一个实例：

```
import 'dart:io';
import 'package:blogserver/blogserver.dart';

main() {
  print("starting");
  var blogServer = new BlogServerApp();
  print("Starting blog server...");

  HttpServer.bind('127.0.0.1', 8080).then((server) {
    server.listen(blogServer.handleRequest);
  });
}
```

服务器启动时向用户提供一些反馈是良好的实践。此服务器为单线程，所有请求都由一个进程以串行方式处理。

## 博客文章的存储格式

博客文章保存在一种简单的文本文件格式中，文件名即为博客文章 ID，并带有 txt 文件扩展名：

```
1st Line - Date
2nd Line - Title
3rd Line until end of file - Post Body
```

1.txt 中的一篇博客文章如下所示：

```
01/05/2015
Giraffe Facts
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>
```

内容是标准的 Lorem ipsum 文本，排版者几个世纪以来一直在使用它。

## 读取文本文件

现在我们已在浏览器之外工作，可以更直接地使用文件系统。让我们看看如何读取文本文件并将每一行打印到标准输出：

```
void main() {
    File myfile = new File("readme.txt");
    myfile.readAsLines().then(
        (List<String> lines)
        {lines.forEach( (line) => print(line) ); }
    );
}
```

要按我们定义的格式读取博客文章，行列表需要进行一些额外处理。这是在 blog.dart 源文件中 BlogPost 构造函数中调用的 process 方法中实现的：

```
BlogPost(String filename, this._id) {
    File postFile = new File(filename);
    _source = postFile.readAsLinesSync();
    process();
}
```

文件使用 readAsLines 的同步版本读取，因此 process 调用无需像前一个示例那样放在 then 处理程序中：

```
process() {
    _html = "";
    _date = _source[0];
    _title = _source[1];
    _html = "<h2>$_title</h2><b>$_date</b><br/>";
    _html += "<img src=\"$_id.png\" align=\"left\">";
    _source.sublist(2).forEach((line) => _html += line);
    _html += "<br/><a href=\"post$_id.html\">Permalink</a>";
}
```

前两行使用 [] 方法选择。剩余的行从列表中拆分为子列表，并直接用于构建将作为输出的 HTML 文件。内容被假定为有效的 HTML。

## 读取文件文件夹

在项目中，content 文件夹的 posts 子文件夹包含所有博客内容。要获取要加载的所有文件的列表，可以使用 `Directory` 对象获取列表，然后使用 `forEach` 进行处理：

```
void _loadPostList() {
    Directory blogContent = new Directory(_pathPosts);
    blogContent.list().forEach((File f) {
        String postFilename =
            path.basename(f.path).replaceAll(".txt", "");
        int id = int.parse(postFilename);
        IDs.add(id);
        postPaths[id] = f.path;
    }).then((v) {
        IDs.sort();
        IDs = IDs.reversed.toList();
    });
}
```

博客文章的文件路径存储在 `Map` 中，以便可以通过 `id` 值检索。`IDs` 存储在列表中，然后可以排序（文件系统无法保证按任何特定顺序提供文件名列表），并且我们可以轻松检索帖子，最近发布的帖子排在最前面。

## 请求处理

传入的 `HttpRequest` 对象包含有关客户端的大量信息——所做的请求和到服务器的网络连接：

```
handleRequest(HttpRequest request) {

    if (request.uri.path.endsWith(".html")) {
        _serveTextFile(request);
    }
    else if (request.uri.path.endsWith(".png")) {
        _servePngFile(request);
    }
    else if (request.uri.path == "/robots.txt") {
        _serveRobotsFile(request);
    }
    else {
        _serve404(request);
    }
}
```

在博客服务器的上下文中，我们最感兴趣的是 `request` 对象的 **Uri（统一资源标识符）** 属性，更 commonly 称为 URL，它给出所请求资源的路径。

例如，如果发送到服务器的请求是 `http://127.0.0.1:8080/post6.html?test=false`（查询字符串不被博客服务器使用），则 `request.uri` 的值将为 `/post6.html?test=false`，而 `request.uri.path` 的值将为 `/post6.html`。

对于正在请求的页面、图像或文件，`request.uri.path` 属性提供了所需详细信息。对 `robots.txt` 文件的请求与博客文章和图像的处理方式不同，这将在后面的章节中详细介绍。

## 提供文本

如果请求页面，我们需要向发出请求的客户端提供反馈。我们的响应首先是告诉客户端我们将响应的数据类型格式（内容类型），也称为 MIME 类型。其次，我们希望让客户端知道我们已理解请求（`HttpStatus.OK`），并即将发送请求的内容：

```
void _serveTextFile(HttpRequest request) {
    String content = _getContent(request.uri.path.toString());

    request.response
        .headers.set('Content-Type', 'text/html')
        .statusCode = HttpStatus.OK
        .write("""<html>
<head><title>$BlogTitle</title></head>
<body>
$content
</body>
</html>""")
        .close();
}
```

设置这些项目后，可以使用请求对象的 `response` 属性将页面内容提供给请求客户端。最后调用 `close()` 方法，以便客户端知道服务器已完成发送内容。

MIME 代表 Multipurpose Internet Mail Extensions。它是 IETF 标准，用于指示文件包含的数据类型。随着在线共享的更多数据类型，此列表不断增长。有关更多详细信息，请参阅 http://www.iana.org/assignments/media-types/media-types.xhtml。

## Robots.txt

使用网络浏览器的用户只是网络用户的一种类型。实时互联网上有许多机器人和蜘蛛。为了给非人类访客提供一些指导，robots.txt 标准告诉机器人是否受欢迎，如果是搜索引擎，则如何索引它。对于博客服务器的目的，我们希望欢迎搜索引擎索引内容以带来更多读者。

此响应的文本内容为：

```
User-agent: *
Disallow:
```

robots.txt 请求需要以与普通文本页面相同的方式处理和提供服务：

```
void _serveRobotsFile(HttpRequest request) {
    request.response
    ..statusCode = HttpStatus.OK
    ..headers.set('Content-Type', 'text/html')
    ..write(RobotsTxt)
    ..close();
}
```

页面以正面的 OK HTTP 状态提供服务。

## 渲染单篇博客文章

博客文章将通过永久链接的 URI 在单个页面上提供，例如 http://127.0.0.1:8080/post6.html，并且 _getContent 方法解析 Uri 路径以获取帖子的 ID 号：

```
if (path.startsWith("/post")) {
    String idfromUrl =
        path.replaceFirst("/post", "").replaceFirst(".html", "");
    int id = int.parse(idfromUrl);
    return hostedBlog.getBlogPost(id).HTML;
}
```

