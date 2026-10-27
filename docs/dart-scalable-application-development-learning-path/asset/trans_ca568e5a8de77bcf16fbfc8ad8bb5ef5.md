

创建 Dartium 启动配置并在 Web 浏览器中打开 index.html 文件。运行服务器并启动新的配置以查看以下结果：

```
GET: /data, Data:4, Params:{number: 0} (31:28)
GET: /data, Data:26, Params:{number: 1} (31:30)
GET: /data, Data: , Params:{number: 2} (31:32)
GET: /data, Data: , Params:{number: 3} (31:34)
GET: /data, Data:47, Params:{number: 4} (31:36)
GET: /data, Data:69, Params:{number: 5} (31:38)
GET: /data, Data: , Params:{number: 6} (31:40)
GET: /data, Data: , Params:{number: 7} (31:42)
GET: /data, Data:98, Params:{number: 8} (31:44)
```

轮询请求存在以下缺点：

- 周期性请求消耗带宽和服务器资源
- 服务器必须始终有数据或无数据时都做出响应；因此，客户端会收到大量空字符串

让我们尝试使用 AJAX 长轮询请求来修复它们。

## AJAX 长轮询请求

从客户端角度来看，AJAX 长轮询请求看起来与普通请求相似。它们在服务器端的一个重要区别是，如果服务器没有可供客户端的信息，它会保持请求并等待信息变得可用或等待合适的超时时间，之后再向客户端发送完成的响应。长轮询请求减少了需要发送的数据量，因为服务器只在数据真正可用时才发送数据。

长轮询请求在以下情况下很有用：

- 你的解决方案必须在旧版 Web 浏览器中工作
- 客户端与服务器之间的数据流量很低
- 实现非常简单

使用长轮询请求的优点如下：

- 它适用于所有浏览器
- 它易于开发，并且可以完美地融入遗留代码中而无需重大更改和努力
- 它可以快速检测连接故障并恢复会话以避免数据丢失
- 它对 IP 地址变化具有很强的免疫力，因为请求是短暂的并且其状态是独立存储的
- 它对大多数实时应用程序性能良好，并减少了带宽消耗和服务器资源利用率

使用此请求的缺点如下：

- 客户端必须不断建立连接；如果服务器将信息发送回客户端并且连接被关闭，它必须等待客户端发送下一个请求以打开新连接
- 客户端需要创建额外的连接来向服务器发送数据
- 并行请求存在问题
- 数据量可能会使服务器的下一次更新变得相当过量，如果数据在服务器上到来之前它等待来自客户端的新连接

以下浏览器受长轮询请求支持：

- Chrome 1.0+
- Firefox 0.6+
- Opera 8.0+
- Safari 1.2+
- Internet Explorer 5.0+

要实现持久的 HTTP 连接，你需要在客户端和服务器端进行更改。我们引入 longLasting 方法来管理请求并将其注册，而不是使用 processParams：

```
//...
main() {
    final allUrls = new RegExp('(/.*)');

    HttpServer.bind(urls.serverAddress, urls.serverPort)
    .then((server) {
        print("Server runs on ${server.address.host}:${server.port}");
        new Router(server)
            .serve(urls.dataUrl, method: 'GET').listen(longLasting)
            .serve(allUrls).listen(serveDirectory('', as: '/'))
            .defaultStream.listen(send404);
    });
}
```

```
// Next function was changed to return data only:
String fetchData() {
    return dataGenerator.nextInt(100).toString();
}
```

新的 longLasting 函数显示在以下代码中。它运行两个周期性计时器。第一个模拟基于请求超时的处理。如果每 30 秒没有收集到数据，它会重置第二个计时器并响应空字符串。第二个管理数据的可用性。如果数据在一秒间隔内可用，它会重置第一个计时器并响应收集到的数据：

```
longLasting(HttpRequest request) {
    Timer reqTimer, dataReadyTimer;

    reqTimer = new Timer.periodic(
        new Duration(seconds:30), (Timer t) {
        dataReadyTimer.cancel();
        processParams(request);
    });

    dataReadyTimer = new Timer.periodic(
        new Duration(seconds:1), (Timer t) {
        if (ready.nextBool()) {
            reqTimer.cancel();
            processQueryParams(request, ready:true);
        }
    });
}
```

processQueryParams 函数的更新版本在以下代码中说明。此函数根据 ready 属性的值返回来自 fetchData 的数据或返回空字符串。代码如下：

```
processQueryParams(HttpRequest request, {bool ready:false}) {
    request.response.write(
        "${request.method}: ${request.uri.path}");
    if (ready) {
        request.response.write(", Data:" + fetchData());
    } else {
        request.response.write(", Data:");
    }
    if (request.uri.queryParameters.length > 0) {
request.response.write(", Params:" +
    request.uri.queryParameters.toString());
}
request.response.close();
}
```

让我们看看如何更改 AJAX 轮询请求示例以在客户端组织长轮询请求。你可以通过调用 longPolling 函数按如下方式启动长轮询请求：

```
void main() {
  DivElement log = querySelector("#log");
  int num = 0;
  longPolling(log, num);
}
```

在此函数中，我们发送 AJAX 请求并等待服务器的响应。当响应可用时，我们调用 responseHandler 函数并立即启动新的请求：

```
longPolling(DivElement log, int num) {
  String query = "number=${num++}";
  HttpRequest.getString("$url?$query")
    .then((response) {
      responseHandler(log, response);
      longPolling(log, num);
    });
}
```

创建 Dartium 启动配置，运行服务器，并在 Web 浏览器中打开 index.html 文件以查看以下结果：

```
GET: /data, Data:12, Params:{number: 0} (51:0)
GET: /data, Data:9, Params:{number: 1} (51:2)
GET: /data, Data:60, Params:{number: 2} (51:3)
GET: /data, Data:41, Params:{number: 3} (51:5)
GET: /data, Data:15, Params:{number: 4} (51:7)
GET: /data, Data:0, Params:{number: 5} (51:8)
```

现在，我们减少了带宽消耗和服务器利用率；因此，客户端将主要只接收有效数据。

## Server-Sent Events

另一种基于 AJAX 的技术是 **Server-Sent Events (SSE)**，也称为 **Server Push** 或 **HTTP Streaming**。在这种情况下，客户端通过初始 HTTP 请求向服务器打开连接，并且当有新信息可用时，服务器向客户端发送事件。因此，如果你的客户端的常规功能类似于股票行情或新闻推送，并且它们需要随着时间的推移从服务器获取更新，那么 SSE 技术就是理想的解决方案了。顺便说一下，如果客户端有新信息要发送给服务器，它可以通过新的 HTTP 请求发送。

SSE 在以下情况下很有用：

- 客户端面向接收大量数据
- 解决方案必须在旧版 Web 浏览器中工作

SSE 的优点如下：

- 服务器实现足够简单
- Web 浏览器可以自动重新连接到服务器
- 消息交换的格式足够灵活
- 解决方案基于到服务器的一个永久连接
- 解决方案足够适用于实时应用程序
- 客户端不需要在每次响应后建立新连接
- 服务器解决方案可以基于事件循环

SSE 的缺点如下：

- 它仅从服务器到客户端工作
- Internet Explorer 不支持它
- 它可能被代理服务器阻止
- 不可能从另一个域连接到服务器

以下浏览器受支持：

- Chrome 6.0+
- Firefox 6.0+
- Opera 9.0+
- Safari 5.0+

