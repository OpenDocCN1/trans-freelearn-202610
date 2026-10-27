

EventSource 类用于接收 SSE。该类通过指定的 URL 经 HTTP 连接到服务器，并在不关闭连接的情况下接收服务器事件。事件以 text/event-stream 格式传入。要打开到服务器的连接并开始接收事件，我们通过工厂构造函数创建 EventSource 类的新实例，并将生成事件的资源的 URL 传递给它。连接建立后，我们便等待消息。

> 生成事件的资源的 URL 必须与调用页面的来源（origin）匹配。

在服务器端，每条消息都以一个文本块的形式发送，并以两个换行符结尾。文本数据使用 UTF-8 编码。每条消息由一行或多行文本组成，列出该消息的字段。每个字段由字段名、冒号和该字段值的文本数据表示。以下是一个仅包含数据的消息示例：

```
; this is a comment
data: some text

data: another text
```

在前面的代码中，第一行只是一个注释。所有以冒号字符开头的文本消息都是注释。如果消息不是定期发送的，注释可作为 Keep-Alive（保持连接）机制使用。第二行和第三行仅包含一个带有文本值的数据字段。第三行包含一个额外的换行符，用于终止消息。通过一条消息可以发送多个事件。每个事件都有一个在 event 字段中指定的名称，以及一个值可以是任意字符串的 data 字段。数据也可以是 JSON 格式，如下面的代码所示：

```
event: userLogon
data: {"username": "John", "time": "01:22:45", "text": "Hello World"}

event: userMessage
data: "Any data"
```

你可以在消息中使用不带 event 字段的未命名事件。SSE 实现使用 message 作为未命名事件的名称。每个事件可能有一个 id。在这种情况下，数据可以按如下方式组合：

```
id: 123
data: some text
data: {"text": "Another text}
```

## 客户端到服务器端的通信

让我们看一下示例的服务器端：

```java
//...
main() {
    final allUrls = new RegExp('/(.*)');

    HttpServer.bind(urls.serverAddress, urls.serverPort)
      .then((server) {
        print("Server runs on ${server.address.host}:${server.port}");
        new Router(server)
            ..serve(urls.dataUrl, method: 'GET').listen(processSSE)
            ..serve(allUrls).listen(serveDirectory('', as: '/'))
            ..defaultStream.listen(send404);
      });
}

String fetchData() {
    return dataGenerator.nextInt(100).toString();
}
```

客户端创建的 EventSource 类实例会打开一个连接，该连接在 accept 标头中提交包含 text/event-stream 值的 HTTP 请求。这是服务器开始 SSE 通信的信号。在我们的示例中，我们先发送 logon 事件。服务器与客户端之间的连接保持打开状态，因此我们可以定期发送 message 事件，如下面的代码所示：

```java
processSSE(HttpRequest request) {
    if (request.headers.value(HttpHeaders.ACCEPT) == EVENT_STREAM) {
        writeHead(request.response);
        int num = 0;

        new Timer.periodic(new Duration(seconds: 5), (Timer t) {
            sendMessageEvent(request.response, num++);
        });

        sendLogonEvent(request.response);
    }
}
```

为了允许你发送推送事件，必须禁用 `HttpResponse` 中的输出缓冲。除此之外，你还需要通过标头属性指定内容类型、缓存控制和连接类型，如下所示：

```
writeHead(HttpResponse response) {
    response.bufferOutput = false;
    response.headers.set(HttpHeaders.CONTENT_TYPE, EVENT_STREAM);
    response.headers.set(HttpHeaders.CACHE_CONTROL, 'no-cache');
    response.headers.set(HttpHeaders.CONNECTION, "keep-alive");
}
```

要发送消息，你可以使用 response 的 `writeln` 方法。它会自动为每个字符串附加一个换行符，因此你只需在事件末尾添加一个换行符。最后，response 的 `flush` 方法会将事件推送到客户端，如下所示：

```
sendMessageEvent(HttpResponse response, int num) {
    print("Send Message event");
    response.writeln('id: 123');
    response.writeln('data: {"msg": "hello world", "num": $num, "value": ${fetchData()} }\n');
    response.flush();
}
```

对于 logon 消息，我们创建一个自定义的 userLogon 事件类型。如果通过 EventSource 打开的连接终止，Web 浏览器将自动在约三秒后重新建立到服务器的连接。你可以通过事件的 `retry` 属性更改此值。该值必须是一个整数，指定以毫秒为单位的重新连接时间：

```
sendLogonEvent(HttpResponse response) {
    print("Send Logon event");
    response.writeln('event: userlogon');
    response.writeln('retry: 15000');
    response.writeln('id: 123');
    response.writeln('data: {"username": "John", "role":
        "admin"}\n');
    response.flush();
}
```

## 客户端到服务器端的通信

客户端侧的 SSE 代码实现如下：

```
import 'dart:html';
import 'urls.dart' as urls;

var url = "http://${urls.serverAddress}:${urls.serverPort}${urls.dataUrl}";
String get timeStamp {
  DateTime dt = new DateTime.now();
  return "(${dt.minute}:${dt.second})";
}

responseHandler(DivElement log, String data) {
  DivElement item = new DivElement();
  item.text = data + timeStamp;
  log.insertAdjacentElement('beforebegin', item);
}
```

在前面的代码中，我们使用指定的 URL 创建了一个 EventSource 实例。从现在开始，我们将开始监听来自服务器的事件。我们添加了 open 和 error 事件监听器。EventSource 类通过 readyState 属性的变化向代码通知服务器侧的连接已关闭。message 监听器处理所有未知事件。一个专门用于 userlogon 事件的事件监听器处理可以通过 EventSource 类的 addEventListener 方法添加的这类事件。事件在 data 属性中保存消息信息。服务器端分配的事件标识符可通过 lastEventId 属性获取，如下面的代码所示：

```
main() {
  DivElement log = querySelector("#log");
  EventSource sse = new EventSource(url);
  sse.onOpen.listen((Event e) {
    responseHandler(log, "Connected to server: ${url}");
  });

  sse.onError.listen((Event e) {
    if (sse.readyState == EventSource.CLOSED) {
      responseHandler(log, "Connection closed");
    } else {
      responseHandler(log, "Error: ${e}");
    }
  });
  sse.onMessage.listen((MessageEvent e) {
    responseHandler(log,
      "Event ${e.lastEventId}: ${e.data}");
```

```
});
    sse.addEventListener("userlogon", (Event e) {
        responseHandler(log,
            "User Logon: ${(e as MessageEvent).data}");
    }, false);
}
```

现在，创建一个 Dartium 启动器，运行服务器，并在 Web 浏览器中打开 index.html，即可看到以下结果：

```
Connected to server: http://localhost:8080/data (43:33)
User Logon: {"username": "John", "role": "admin"} (43:33)
Event 123: {"msg": "hello world", "num": 0, "value": 45} (43:38)
Event 123: {"msg": "hello world", "num": 1, "value": 1} (43:43)
Event 123: {"msg": "hello world", "num": 2, "value": 65} (43:48)
Event 123: {"msg": "hello world", "num": 3, "value": 10} (43:53)
//...
```

让我们看一下服务器日志，了解服务器生成的消息：

```
Server runs on 127.0.0.1:8080
Send Logon event
Send Message event 0
Send Message event 1
Send Message event 2
Send Message event 3
//...
```

## WebSocket

WebSocket 是客户端和服务器之间一种双向、面向消息的流式传输。它构建于 TCP 之上，并使用 HTTP 升级握手。WebSocket 是一种基于消息的传输。其简单而极简的 API 抽象了所有复杂性，并免费提供以下额外服务：

- 同源策略强制
- 与现有 HTTP 基础设施的互操作性
- 面向消息的通信
- 子协议协商的可用性
- 低成本的扩展性

