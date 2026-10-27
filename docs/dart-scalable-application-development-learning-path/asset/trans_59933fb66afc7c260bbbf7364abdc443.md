
```
router.serve('/ws').transform(new WebSocketTransformer()).
    listen(handleWebSocket);
});
}
```

作为路由如何有用的一个更一般的例子，考虑以下示例。假设我们的客户端搜索以 /stocks 和 /stocks/GOOG 结尾的 URL 的股票数据。然后，我们可以将 pattern1 和 pattern2 定义为 UrlPattern 类的实例，其正则表达式包含以下模式：

```
// Pattern for all stocks(plural).
final stocksUrl = new UrlPattern(r'/stocks\/?');
// Pattern for a single stock('/stock/GOOG', for example).
final stockUrl = new UrlPattern(r'/stock/(\d+)\/?');
```

然后，我们的路由器实例将通过 serve 方法将回调函数 serveStocks 和 serveStock 绑定到这些模式：

```
var router = new Router(server)
..serve(stocksUrl, method: 'GET').listen(serveStocks)
..serve(stockUrl, method: 'GET').listen(serveStock)
    // all other possible patterns and method combinations
..defaultStream.listen(serveNotFound);
```

如第一个示例所示，模式也可以是简单的字符串，如 /stockdata。

## 另请参阅

- 有关 runZoned 的更多信息，请参阅使用 http_server 的“提供文件服务”指南。
- Dart 网站提供了一个关于使用 WebSockets 和 Dartiverse Search 实现的搜索应用的非常好的教程，网址为 https://www.dartlang.org/docs/dart-up-and-running/contents/ch05.html

## 使用安全套接字和服务器

在本指南中，我们将介绍如何使您的 Web 服务器使用 HTTPS 协议上的安全套接字层（SSL）加密与客户端的通信。

## 准备工作

Dart 使用 SSL/TLS 安全机制；它依赖 X.509 证书来验证服务器和（可选）客户端。服务器提供一个证书，向客户端证明自己是受信任的服务器。当客户端接受该证书时，将交换对称会话密钥，用于加密服务器与客户端之间的通信。因此，为了让您的服务器提供安全连接，它必须安装由证书颁发机构（CA）提供的安全证书。

Dart 使用网络安全服务（NSS）数据库来存储服务器的私钥和证书。在我们的示例中，我们将使用子文件夹 pkcert 中的测试证书数据库，该数据库也作为示例在 https://www.dartlang.org/docs/tutorials/httpserver/ 的教程中提供。

您可以自行设置 NSS 密钥数据库来创建用于测试目的的证书。James Locum 在 http://jamesslocum.com/post/70003236123 提供了如何执行此操作的详细说明。

## 操作步骤...

程序 secure_server.dart 向我们展示了启动安全服务器所需的代码；执行以下步骤以使用安全套接字和服务：

+   1. 导入 dart:io 包，如下所示：

```
import 'dart:io';

InternetAddress HOST = InternetAddress.LOOPBACK_IP_V6;
const int PORT = 8080;

main() {
```

+   2. 使用以下代码读取证书：

```
var testcertDb =
  Platform.script.resolve('pkcert').toFilePath();
SecureSocket.initialize(database: testcertDb, password:
  'dartdart');
```

+   3. 使用以下代码使用证书启动 HTTP 服务器：

```
HttpServer.bindSecure(HOST, PORT, certificateName:
  'localhost_cert').then((server) {
  print('Secure Server listening');
  server.listen((HttpRequest req) {
    print('Request for ${req.uri.path}');
    var resp = req.response;
    resp.write("Don't worry: I encrypt your messages!");
```

```
resp.close();
    });
  });
}
```

4. 如果我们现在在浏览器中使用 URL https://localhost:8080，首先会看到一个警告屏幕，提示此连接不受信任（因为这只是测试证书）。如果我们继续，我们会在浏览器屏幕上看到服务器的响应，如下面的截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_748_0.png)

以下是安全命令行客户端（secure_client.dart）的代码：

```
import 'dart:io';

InternetAddress HOST = InternetAddress.LOOPBACK_IP_V6;
const int PORT = 4777;
SecureSocket socket;

void main() {
  SecureSocket.connect(HOST, PORT, onBadCertificate:
      (X509Certificate c) {
    print("Certificate WARNING: ${c.issuer}:${c.subject}");
    return true;
  }).then(handleSecureSocket);
}

handleSecureSocket(SecureSocket ss) {
  // send to server:
  ss.write("From client: can you encrypt me server?");
  // read from server:
  ss.listen((List data) {
    String msg = new String.fromCharCodes(data).trim();
    print(msg);
  });
}
```

客户端控制台给出以下输出：

**证书警告：CN=myauthority:CN=localhost**

## 工作原理...

在步骤 1 中，我们读取证书。带有 `Platform.script` 的第一行找到文件夹 `pkcert` 的路径，证书数据库位于该文件夹中。然后我们在 `SecureSocket` 类上调用 `initialize` 方法，提供证书。在步骤 2 中，通过绑定到主机和端口并提供证书名称来启动安全服务器。步骤 3 向我们展示了浏览器连接到安全服务器。

在步骤 4 中，我们看到了命令行客户端如何通过调用 `SecureSocket.connect()` 连接到安全服务器。这需要一个 `onBadCertificate` 回调，该回调必须返回一个布尔值，指示是接受还是拒绝无效证书。测试证书将触发此回调，因此我们需要返回 `true` 才能使用此证书。对于 `SecureSocket` 的 `write` 和 `listen` 方法，让我们向安全服务器写入和读取。

## 另请参阅

- 有关 `Platform` 类的更多详细信息，请参阅第 1 章 *使用 Dart 工具* 中的 *从操作系统获取信息* 指南。
- 有关证书及其创建的更多信息，请参阅 https://help.ubuntu.com/12.04/serverguide/certificates-and-security.html

## 使用 JSON Web 服务

在本指南中，我们让一个浏览器应用以 JSON 格式从 Web 服务（Yahoo 股票数据）请求数据，解码该数据，并动态构建显示数据的网页。

## 准备工作

我们将使用的 URL 如下所示：http://query.yahooapis.com/v1/public/yql?q=SELECT。

为了获取数据，我们使用 **Yahoo 查询语言（YQL）**，q= 表示由 SELECT 表示的查询的开始。假设我们要查找 Yahoo、Google、Apple 和 Microsoft 的股票数据，所选查询将采用以下形式：

```
select * from yahoo.finance.quotes where symbol
in(YHOO,AAPL,GOOG,MSFT)
&env=http://datatables.org/Falltables.env&format=json
```

## 使用 Web 服务器

## 操作步骤...

查看 `stockviewer.dart` 中的代码：

```
import 'dart:html';
import 'dart:convert';

main() {
  LoadData();
}
```

+   1. 使用以下代码异步调用 Web 服务器：

```dart
void LoadData() {
  var stock = "GOOG";
  var request =
      "http://query.yahooapis.com/v1/public/"
      "yql?q=select%20*%20from%20yahoo.finance.quotes%20"
      "where%20symbol%20in%20(%22$stock%22)%0A%09%09"
      "&env=http%3A%2F%2Fdatatables.org%2Falltables."
      "env%3A%3A%20json";
  var result =
      HttpRequest.getString(request).then(OnDataLoaded);
}
```

+   2. Web 服务响应回调，如下面的代码所示：

```dart
void OnDataLoaded(String response) {
  String json = response.substring(response.indexOf("symbol")
      - 2, response.length - 3);
  Map data = JSON.decode(json);
  var table = CreateTable();
  var props = data.keys;
  props.forEach((prop) => ProcessStockEntry(prop, data,
      table));
  document.body.nodes.add(table);
}
```

3. 使用数据创建 HTML 表格，如下面的代码所示：

```
TableElement CreateTable() {
    TableElement table = new TableElement();
    var tBody = table.createTBody();
    return table;
}

void ProcessStockEntry(String prop, Map data, TableElement table) {
    String value = data["$prop"];
    var row = table.insertRow(-1);  // Add new row to our table
    var propCell = row.insertCell(0); // Add new cell for property
    String prophtml = '$prop:';
    propCell.setInnerHtml(prophtml);
    var valueCell = row.insertCell(1); // Add new cell for the value
    String valuehtml = '$value';
    valueCell.setInnerHtml(valuehtml);
}
```

浏览器显示股票数据，如下面的截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_751_0.png)

来自 JSON Web 服务的股票数据
