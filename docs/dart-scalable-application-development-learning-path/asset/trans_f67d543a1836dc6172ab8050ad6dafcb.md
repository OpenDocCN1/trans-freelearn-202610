

## 客户端到服务器通信

WebSocket 是浏览器中最灵活的传输方式之一。API 实现了该层，并在客户端与服务器之间以流式方式传递任意应用协议，并且可以在任一端随时发起。

> WebSocket 是一组多个标准：WebSocket API 和 WebSocket 协议及其扩展。

WebSocket 可用于以下情况：
- 当数据的相关性非常关键时
- 当解决方案非常经常基于海量数据或数据传输时

WebSocket 的优点如下：
- 它是一个全双工、双向通信通道，通过单个套接字运行
- 它基于套接字协议提供快速的文件交换
- 它支持二进制格式

WebSocket 的缺点如下：
- 它不是 HTTP，可能会被代理服务器阻止
- 调试比较复杂
- 它仅被所有浏览器的现代版本支持

以下浏览器受支持：
- Chrome 14.0+
- Firefox 11.0+
- Opera 8.0+
- Safari 6.0+
- Internet Explorer 10.0+

请记住，WebSocket 使用自定义的 WS 协议而不是 HTTP。
WebSocket 协议的用例是为在浏览器中运行的应用与服务器之间提供优化的、双向通信通道，而使用 HTTP 协议是显而易见的。WebSocket 使用自定义 URL 方案，因为 WebSocket 线路协议可以在浏览器之外使用，并且可以通过非 HTTP 交换建立，而双向或服务器发起的 HTTP（HyBi）工作组选择采用自定义 URL 方案。让我们来看一下我们示例的客户端代码：

```
import 'dart:html';
import 'dart:convert';
import 'dart:typed_data';
import 'urls.dart' as urls;

var url = "ws://${urls.serverAddress}:${urls.serverPort}${urls.dataUrl}";

String get timeStamp {
  DateTime dt = new DateTime.now();
  return " (${dt.minute}:${dt.second})";
}

responseHandler(DivElement log, String data) {
  DivElement item = new DivElement();
  item.text = data + timeStamp;
  log.insertAdjacentElement('beforebegin', item);
}
```

首先，我们创建一个 WebSocket 类实例以使用指定的 URL 发起新的连接。然后，我们为 open、error、close 和 message 事件创建监听器。正如你所见，API 看起来与我们在上一主题中看到的 EventSource API 非常相似。这是故意的，因为 WebSocket 提供类似的功能，并且可以帮助快速从 SSE 解决方案过渡。
当连接建立后，我们可以向服务器发送数据。由于 WebSocket 对应用负载没有任何假设和约束，我们可以发送任何数据类型，例如 Map、String 和 Typed：

```
void main() {
  DivElement log = querySelector("#log");
  var webSocket = new WebSocket(url);
  if (webSocket != null) {
```

```
responseHandler(log, "Connected to server: ${url}");
sendData(webSocket);
});
webSocket.onError.listen((Event e)
    => responseHandler(log, "Error: ${e}") );
webSocket.onClose.listen((CloseEvent e)
    => responseHandler(log, "Disconnected from server"));
webSocket.onMessage.listen((MessageEvent e)
    => responseHandler(log, "Event ${e.type}: ${e.data}"));
}
}
sendData(WebSocket webSocket) {
  webSocket.send(JSON.encode({'name':'John', 'id':1234}));
  webSocket.sendString("Hello World");
  webSocket.sendTypedData(new Int16List.fromList([1, 2, 3]));
}
```

浏览器会自动将接收到的基于文本的数据转换为 DOMString 对象，并将二进制数据转换为 Blob 对象，然后直接传递给应用。我们可以通过 WebSocket 类的 binaryType 属性强制进行 arrayBuffer 转换，当接收到二进制消息时：

```
webSocket.binaryType = "arraybuffer";
```

这是向用户代理提示如何根据 binaryType 中的值处理传入二进制数据的完美提示：
- 如果它等于 `Blob`，则保存到磁盘
- 如果它等于 `arrayBuffer`，则保留在内存中

Blob 对象中的数据是不可变的，表示原始数据。它可能是保存从服务器下载的图像的最佳格式。我们可以直接将该数据传递给图像标签。另一方面，arrayBuffer 对象更适合对二进制数据进行额外处理。在我们的示例中，我们使用了 UTF-8 编码的文本消息、UTF-8 编码的 JSON 有效负载以及二进制有效负载的 arrayBuffer 对象。WebSocket 类的所有发送方法都是异步的，但它们以客户端排队的确切顺序传递。因此，队列中大量消息的传递会被延迟。为了解决这个问题，应用可以将大消息拆分为小块，并通过 WebSocket 类的 bufferingAmount 属性监控发送数据，如下所示：

```
if (webSocket.bufferingAmount == 0) {
  ws.send(nextData);
}
```

## 总结

为了总结到目前为止所讨论的内容，我想强调客户端到服务器通信的一些重要方面。基于消息交换数据以使客户端和服务器之间进行数据交换的特殊规则和格式的系统称为通信协议。连接到互联网的任何设备都有唯一的 IP 地址。从一台设备传输到另一台设备通过互联网的消息会遵循长路由通过协议栈传递。HTTP 是一种基于文本的应用协议，使网络工作。Web 浏览器或独立应用发送请求并打开到 Web 服务器的连接。Web 服务器遵守请求并关闭到 Web 客户端的连接。

AJAX 是一种技术，允许客户端 JavaScript 代码在不重新加载 Web 浏览器中当前页面的情况下从服务器请求数据。`HttpRequest`（以前称为 `XMLHttpRequest`）在 AJAX Web 开发技术中起着重要作用。AJAX 轮询请求会定期发出请求以获取新信息。长轮询请求始终保持与服务器的打开连接。该连接保持活动状态，直到服务器决定将信息提交回客户端，其中有更改，然后关闭连接。客户端将再次打开与服务器的连接以开始新的长轮询请求。

在 SSE 中，客户端通过初始 HTTP 请求打开与服务器的连接，并且当有新信息可用时，服务器向客户端发送事件。通过 SSE 发送回的响应是使用 `text/event-stream` 内容类型提供的纯文本。它包含一个或多个以 `data:` 字符串开头并以换行符结束的字符串行。最后，整个消息以额外的换行符结束。

WebSocket 是客户端与服务器之间的双向、面向消息的流式传输。它建立在 TCP 之上，使用 HTTP 升级握手。API 实现了该层，并在客户端与服务器之间以流式方式传递任意应用协议，并且可以在任一端随时发起。

在下一章中，我们将讨论在客户端本地存储数据并突破 Web 应用中 cookie 存储限制的能力。我们还将演示如何使用 Web Storage 并探索更强大、更有用的 IndexedDB 以在用户的 Web 浏览器中存储大量数据。

## 高级存储

Cookie 由 Web 浏览器透明处理。如果其 URL 匹配域并且 cookie 中指定的路径匹配，则它们会添加到每个 HTTP 请求的标头中。Cookie 可通过 `document.cookie` 属性访问。请记住，当你创建 cookie 时，你以特殊文本格式指定 cookie 的名称、值、到期日期，以及可选的路径和域。你只能按名称查找 cookie 并获取值，因为在创建 cookie 时指定的所有其他信息都由 Web 浏览器使用，对你不可用。我们创建了 `cookies.dart` 文件以保留原始 cookie API 的小型库包装器，以避免样板代码和代码重复，这允许你轻松地从任何应用中设置、获取和删除 cookie。Cookie 使用源自 RFC 1123 国际格式的日期格式，即 Wdy, DD Mon YYYY HH:MM:SS GMT。让我们更详细地看一下：
- **Wdy**：这是代表星期几的三个字母缩写，后跟一个逗号
- **DD**：这是代表月份中某一天的两个数字
- **Mon**：这是代表月份名称的三个字母
- **YYYY**：这是代表年份的四个数字
- **HH**：这是代表小时的两个数字
- **MM**：这是代表分钟的两个数字
- **SS**：这是代表秒的两个数字

你可以在此示例中看到日期：Thu, 09 Oct 2014 17:16:29 GMT。

让我们通过以下代码并看看 Dart 如何访问 cookie。我们特别导入 `dart:html` 包以使 `document.cookies` 在我们的代码中可用。`intl.dart` 包是因 `toUTCString` 函数中使用 DateFormat 而导入，用于根据 days 属性中的值计算 cookie 的 UTC 格式到期日期并将其转换为字符串。如果 days 的值小于一，则 toUTCString 函数返回空字符串。要使用 setCookie 函数创建 cookie，我们需要指定 cookie 的名称、值和到期天数。我们也可以提供可选的路径和域信息。在实践中，你根本不能在 cookie 中使用非 ASCII 字符。要使用 Unicode、控制代码或其他任意字节序列，你必须使用最流行的 UTF-8-inside-URL 编码，该编码由 Uri 类的不同编码方法生成。要通过 getCookie 返回 cookie，我们只需要 cookie 的名称。最后，你会找到 removeCookie 函数。

```
library cookie;

import 'dart:html';
import 'package:intl/intl.dart';
```
