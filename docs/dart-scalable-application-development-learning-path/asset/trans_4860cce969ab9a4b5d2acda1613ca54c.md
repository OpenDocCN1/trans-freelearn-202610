
> `whenComplete` 方法是与 `finally` 块等价的异步实现。

现在我们已经完成了定义，接下来讨论 `Future` 的不同工厂构造函数。

## Future 与 Timer

让我们使用 `Timer` 类的 `run` 方法异步创建一个包含调用计算结果的新 `Future` 类，如下所示：

```
Future calc = new Future(computation);
calc.then((res) => print(res));
```

这个 `Future` 类不会立即完成。`Timer` 类会将事件添加到事件队列中，并在事件循环处理该事件时执行计算回调。如果计算函数的结果抛出错误，返回的 `Future` 会以错误状态完成。如果计算函数创建了另一个 `Future`，当前 `Future` 会等待新的 `Future` 完成，然后以相同的结果完成。

## Future 与微任务（Microtask）

在以下代码中，`Future` 类是微任务队列中的一个调度任务，不会立即完成：

```
Future calc = new Future.microtask(computation);
calc.then((res) => print(res));
```

如果计算结果抛出异常，返回的 `Future` 会以该错误完成。如果计算过程创建了另一个 `Future`，当前 `Future` 会等待新的 `Future` 完成，然后以相同的结果完成。

## 同步 Future 类

这听起来可能有些矛盾，但我们仍然可以创建 `Future` 类的同步版本，如下所示：

```
Future calc = new Future.sync(computation);
calc.then((res) => print(res));
```

原因是 `Future` 会立即调用计算函数。计算结果将在下一个事件循环迭代中返回。

## 带值的 Future

`Future` 类可以通过指定值来创建，如下所示：

```
Future valueFuture = new Future.value(true);
valueFuture.then((res) => print(res));
```

在这里，`Future` 会在下一个事件循环迭代中返回指定值。

## 带错误的 Future

`Future` 类可以通过指定错误来创建，如下所示：

```
try {
    throw new Error();
} on Error catch(ex, stackTrace) {
    Future errorFuture = new Future.error(ex, stackTrace);
    errorFuture.catchError((err, stack) => print(err));
}
```

这个 `Future` 会在下一个事件循环迭代中以错误状态完成。

## 延迟 Future 类

有时可能需要延迟完成 `Future`。可以按如下方式实现：

```
Future calc = new Future.delayed(
    new Duration(seconds: 1), computation);
calc.then((res) => print(res));
```

`Future` 会在指定的持续时间过后，以计算函数的结果完成。它始终会在事件队列中创建一个事件，如果持续时间小于或等于零，该事件也不会早于下一个事件循环迭代完成。

> `Future` 类不得更改已完成的数值或错误，以避免对监听器产生副作用。

如果 `Future` 没有后继者，任何错误都可能会被静默丢弃。为了防止这些情况，Dart 通常会将错误转发给全局错误处理器。

那么，如何解决这个问题呢？我们将代码包装在 try/catch 块中，以捕获所有未捕获的异常，这种方法非常有效，如下列代码所示：

```
main() {
    try {
        runServer();
    } on Error catch(e) {
        // ...
    }
}
```

这个解决方案具有通用性，可用于类似场景，因此我们将通过创建单独的包装函数来对其进行通用化：

```
wrapper(Function body, {Function onError}) {
    try {
        body();
    } on Error catch(e) {
        if (onError != null) {
            onError(e);
        }
    }
}

main() {
    wrapper(runServer, onError: (e) {
        // ...
    });
}
```

`body` 参数代表任何受保护的代码，并被包含在 `wrapper` 内部的 try/catch 块中。`wrapper` 函数使用 `onError` 函数来处理所有未捕获的异常。使用包装函数是一种良好的实践，建议在其他类似场景中也使用它。这就是 zone（区域）。

> zone（区域）是一种可配置的执行上下文，用于处理未捕获的异常和异步任务。

让我们看看 zone 可以做什么：

- 在关键情况下，它允许你妥善处理异常
- 它提供了一种在单个组中处理多个异步操作的方法
- 它可以拥有无限数量的嵌套 zone，其行为与父 zone 相同

每个 zone 都会创建一个上下文，一种受保护的区域，使执行中的代码存在于其中。除了拦截未捕获的异常外，zone 还可以拥有局部变量，并可以调度微任务、创建一次性或重复定时器、打印信息，以及保存堆栈跟踪以用于调试。

## 简单的 zone 示例

让我们将代码改为使用 zone 来代替包装函数，如下所示：

```
import 'dart:io';
import 'dart:async';

main() {
    runZoned(runServer, onError: (e) {
        // ...
    });
}
```

`runZoned` 函数是一个代码包装器。默认情况下，async 库会隐式创建一个根 zone，并将其分配给 `Zone` 类中的一个静态 `current` 变量。因此，我们在 `runZoned` 函数内总是可以获得一个活动 zone。当 `runZoned` 函数运行时，它会从根 zone 分叉出新的嵌套 zone，并在其上下文中执行 `runServer` 函数。使用当前 zone 的 `fork` 方法可以创建它的一个新子 zone。

> 只能通过当前 zone 的 `fork` 方法创建 zone。

## zone 嵌套

假设我们需要在服务器中提供静态文件服务。那么，我们将需要读取文件并提供服务。为了正确实现这一点，我们分叉出嵌套 zone，并使用 `runZoned` 函数保护我们的代码，如下所示：

```
runServer() {
    HttpServer
    .bind(InternetAddress.ANY_IP_V4, 8080)
    .then((server) {
        server.listen((HttpRequest request) {
            runZoned(() {
                readFile(request.uri.path).then((String content) {
                    request.response.write(content);
                    request.response.close();
                });
            }, onError: (e) {
                request.response.statusCode = HttpStatus.NOT_FOUND;
                request.response.write(e.toString());
                request.response.close();
            });
        });
    });
}

Future<String> readFile(String fileName) {
  switch (fileName.trim()) {
    case "/":
    case "/index.html":
    case "/favicon.ico":
      return new Future.sync(() => "Hello, world!");
  }
  return new Future.sync(() =>
    throw new Exception('Resource is not available'));
}
```

在嵌套 zone 内部，我们使用资源名称调用 `readFile` 函数，它会返回内容。如果资源不可用，`readFile` 会产生异常，程序会在 `onError` 函数中捕获该异常，该函数被注册为 zone 的错误处理器。如果我们不指定错误处理器，异常将沿着 zone 嵌套层次向上传播，直到某个父级 zone 捕获它，或者它到达顶层可执行代码并终止程序。

## zone 值

现在，我们来讨论服务器上的身份验证问题，因为某些资源可能不对公众开放。我们将采用基于令牌的身份验证思路，该思路依赖在每个请求中发送到服务器的签名令牌。我们将创建一个令牌集合来记录所有已授权的客户端，然后分叉出一个新的 zone 进行身份验证。随后，我们将从请求头中读取用于身份验证的客户端令牌。当我们从当前 zone 获取令牌集合时，我们会通过 `zoneValues` 将其注入到 zone 中，如下列代码所示：

```
runServer() {
  HttpServer
  .bind(InternetAddress.ANY_IP_V4, 8080)
  .then((server) {
    Set tokens = new Set.from(['1234567890']);
    server.listen((HttpRequest request) {
        runZoned(() {
            authenticate(request.headers.value('auth-token'));
        }, zoneValues: {'tokens': tokens}, onError: (e) {
            request.response.statusCode = HttpStatus.UNAUTHORIZED;
            request.response.write(e.toString());
            request.response.close();
        });
        runZoned(() {
            readFile(request.uri.path).then((String content){
                request.response.write(content);
                request.response.close();
            });
        }, onError: (e) {
            request.response.statusCode = HttpStatus.NOT_FOUND;
            request.response.write(e.toString());
            request.response.close();
        });
    });
  });
}
```

基于令牌是否存在于 `tokens` 集合中的身份验证如下所示：

```
authenticate(String token) {
    Set tokens = Zone.current['tokens'];
    if (!tokens.contains(token)) {
        throw new Exception('Access denied');
    }
}
```

在上述代码中，我们使用 zone 局部变量来跟踪令牌并验证客户端。在这里，变量通过 `runZoned` 函数的 `zoneValues` 参数被注入到 zone 中。我们的 `tokens` 变量在异步上下文中的作用类似于静态变量。

> zone 局部变量可以扮演静态变量的角色，仅在该 zone 的范围内可见。
