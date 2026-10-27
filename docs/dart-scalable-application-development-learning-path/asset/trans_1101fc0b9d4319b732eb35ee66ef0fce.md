
## 定位文件

Blog 类的 `getBlogImage` 方法返回用于在博客文章中显示的 PNG 文件。当构造函数中调用 `_loadImgList` 时，Blog 类会构建一个图像文件名列表。这与之前描述的 `_loadPostList` 函数的方式相同：

```dart
File getBlogImage(int index) {
    return new File(imgPaths[index]);
}
```

该 ID 被用作从映射中获取值的键，并返回完整的文件路径。Web 服务器指向文件的 Uri 与文件系统的位置完全无关，由应用程序的逻辑决定将哪个文件传送给客户端。

## 提供单个图像文件

服务器并不知道为 index.html 提供的网页（HTML 和图像）的完整结构。它只是以单一的方式返回对 Web 浏览器 HTTP 请求的响应：

![](img/52c05b53a56abd53584e4fed738ed509_125_0.png)

单个图像可以在 HTML 页面之外直接请求。

## 提供 404 错误

即使是非技术性的 Web 用户也熟悉 404 错误，它表示网站上的页面无法找到：

```dart
void _serve404(HttpRequest request) {
  request.response
    ..statusCode = HttpStatus.NOT_FOUND
    ..headers.set('Content-Type', 'text/html')
    ..write(page404)
    ..close();
}
```

此页面是一个普通文本页面（常量字符串 page404 包含内容），并通过将状态码设置为 NOT_FOUND 进一步告知客户端 404 状态，如下面的截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_126_0.png)

希望博客服务器不需要提供太多这样的页面！

## 介绍 Dart 的服务器框架

本项目中的博客服务器直接基于 dart:io 编写，因此你可以了解其内部工作原理。正如你所期望的，Dart 有许多框架可以帮助编写服务器应用程序，这样基本任务（如提供文本文件）就不必为每个应用程序从头实现。

这些都可以在 pub 包库中找到。

### Redstone

Redstone 被描述为“Dart 平台的服务端微框架”。它使用简单的代码注解将 Dart 函数暴露给 Web。Redstone 的 Hello World 示例非常清晰，展示了注解如何向开发者隐藏复杂性，使他们能够专注于应用程序逻辑：

```dart
import 'package:redstone/server.dart' as app;

@app.Route("/")
helloWorld() => "Hello, World!";

main() {
  app.setupConsoleLog();
  app.start();
}
```

> 你可以在 http://redstonedart.org/ 了解更多关于 Redstone 的信息。

### Rikulo

Rikulo 谦虚地将自己描述为一个轻量级的 Dart Web 服务器。它拥有一系列有趣的功能，包括请求路由、模板引擎和 MVC（模型-视图-控制器）设计模式。Rikulo Stream 是服务器端的解决方案，其提供静态内容的 Hello World 示例只有一行：

```dart
import "package:stream/stream.dart";
void main() {
  new StreamServer().start();
}
```

> 你可以在 http://rikulo.org/ 了解更多关于 Rikulo 的信息，那里列出了几个项目。

### Shelf

Shelf 声称它可以轻松创建和组合 Web 服务器以及 Web 服务器的各个部分。其基于管道的模型允许添加中间件（例如其 Hello World 示例中的请求记录器）：

```dart
import 'package:shelf/shelf.dart' as shelf;
import 'package:shelf/shelf_io.dart' as io;

void main() {
  var handler = const shelf.Pipeline().addMiddleware(shelf.logRequests())
    .addHandler(_echoRequest);

  io.serve(handler, 'localhost', 8080).then((server) {
    print('Serving at http://${server.address.host}:${server.port}');
  });
}

shelf.Response _echoRequest(shelf.Request request) {
  return new shelf.Response.ok('Request for "${request.url}"');
}
```

> 在 https://github.com/dart-lang/shelf 了解更多关于 Shelf 的信息，正如这个 URL 所暗示的，这个包来自 Dart 团队。

## 部署

在一个漫长而艰难的软件开发项目之后，如果最后一刻出现意外，要让最终产品在正式系统上运行可能会带来极大的压力。最好在开发过程的早期就考虑部署，以便应用程序适合目标环境。

我们将了解如何在两种常见环境中部署 Dart——Windows 和 Unix。当然，每个具体系统在细节上可能有所不同，因此你可能需要检查系统上等效的设置或程序，并考虑到任何当前正在运行的应用程序，例如 Web 服务器。

### 依赖项

Dart SDK 本身是任何 Dart 服务器应用程序的关键依赖。应用程序通过 dart 命令行应用程序运行，并将 main.dart 脚本作为参数传递。

对于交互式会话来说，这很简单；然而，对于服务器应用程序，我们需要在没有用户登录到机器时运行该应用程序。

> Dart SDK 可作为单独下载（不包括开发人员工具），网址为 https://www.dartlang.org/downloads/。

### 在 Unix 上部署

在 Linux 或其他类 Unix 系统上，Web 应用程序可以在名为 screen（小写 s）的控制台应用程序中启动。screen 命令允许创建其他控制台会话，一旦主会话结束，这些会话可以在后台继续运行。

可以创建一个 screen 应用程序——应用程序启动后，screen 被分离，主会话结束。然后 Web 应用程序继续运行。如果需要进行更改，可以重新连接已分离的 screen（这可能是在几个月或几年之后！）进行任何调整；例如，安装新版本或更改配置。

这可以用于允许直接在服务器上进行 shell 访问（通常是 SSH）的托管系统。

### 使用 screen 命令

以适当的用户身份登录系统上的交互式 shell。要检查是否安装了 screen，请运行以下命令以获取版本和描述：

```
Screen
Screen version 4.01.00devel (GNU) 2-May-06

Copyright (c) 2010 Juergen Weigert, Sadrul Habib Chowdhury
Copyright (c) 2008, 2009 Juergen Weigert, Michael Schroeder
...
```

如果程序未安装，请使用你的包管理器安装该包，它简单地称为 screen。

### 启动 screen

一旦 screen 应用程序运行，按 Enter 键，你将再次进入命令提示符。按 Ctrl+a，将显示版本屏幕，表明我们不在正常的 shell 中。

使用 cd 进入 BlogServer 文件夹，并启动 Dart 博客服务器：

```
dart bin/main.dart
```

接下来，按 Ctrl+a、Ctrl+d，你将看到类似以下的消息：

```
[detached from 398.pts-2.localhost]
```

现在你应该回到常规终端了。运行 top 命令（或任何其他列出系统上正在运行的进程的程序）来获取进程列表，你会看到 Dart 在后台运行。

运行 Web 浏览器并导航到 index.html 页面。博客将从编辑器调试会话中提供。

要重新连接到这个 screen，使用命令 screen -r，你将回到该会话中。

> screen 命令是一个强大的工具，但要记住哪个 screen 正在使用可能会有点令人困惑。建议你将键盘快捷键牢记于心，以便于导航。

### 在 Windows 上部署

在 Windows 上，服务器应用程序通常作为 Windows 服务运行，对于 .NET 应用程序，则通过 Internet Information Services 托管。

将 Dart bin/main.dart 命令放入批处理文件中是一项相对简单的操作，这样它可以在交互式会话中双击运行。如果机器意外重启或应用程序崩溃，最好消除需要用户登录的要求。

### 使用 NSSM 工具

免费工具 NSSM（Non-Sucking Service Manager）（可在 http://nssm.cc/ 获取）具有一系列管理服务的功能。其中之一是将任何应用程序安装为服务，如下面的截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_131_0.png)

DartBlog 服务已安装并设置为自动启动，并在 Local System 账户下运行：

![](img/52c05b53a56abd53584e4fed738ed509_131_1.png)

请注意，服务在首次安装时处于停止状态。根据应用程序的功能，服务将需要在适当的用户账户下运行，例如写入文件或执行其他操作。
