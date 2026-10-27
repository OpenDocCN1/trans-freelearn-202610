

Isolates 开始时通过交换 SendPort 来实现通信。两种方法都返回一个包含 isolate 的 Future，或者类型为 IsolateSpawnException 的错误，必须捕获。这可以通过链式调用 catchError 子句或使用 spawn 的可选 onError 参数来完成。然而，在已启动的 isolate 中发生的错误无法被主 isolate 捕获，这很合理，因为两者都是独立执行的代码集。你可以通过运行 isolates_errors.dart 来自行验证。使用 spawn 和 spawnUri 时请注意以下限制：

- Spawn 在服务器应用中有效，但在 Dart Web 应用中无效。浏览器的主 isolate，DOM isolate，不允许这样做。这是为了防止对 DOM 的并发访问。
- 然而，spawnUri 在 Dart Web 应用和服务器应用中都有效，并且由此产生的 isolate 本身也可以启动其他 isolate。Dart VM 将这些 isolate 转换为 Web Worker（请参阅 http://www.html5rocks.com/en/tutorials/workers/basics/）。

### There's more....

因此，如果你需要在应用内运行一个计算密集型任务，为了保持应用的响应性，你应该将该任务放入自己的 isolate 或 worker 中。如果有许多此类任务，那么应该部署多少个 isolate？一般来说，当任务是计算密集型时，你应该使用与预期可用的核心或处理器数量一样多的 isolate。纯计算的额外 isolate 是冗余的。但如果其中一些还执行异步调用（例如 I/O），那么它们不会使用太多处理器时间。在这种情况下，拥有比处理器更多的 isolate 是有意义的；一切都取决于你的应用架构。在极端情况下，你可以为每个功能使用一个单独的 isolate，或确保数据不被共享。

![](img/52c05b53a56abd53584e4fed738ed509_770_0.png)

> 始终对你的应用进行基准测试，以检查 isolate 数量是否已针对任务进行了优化。你可以按如下方式进行：在 Run Manage Launches 工具中，勾选 VM 设置中的选项，pause isolate on start，以及 pause isolate on exit。然后，通过 Dart Editor 左侧的图像按钮打开 observatory 工具到终止的红色方块，在那里你可以找到有关分配和性能的有趣信息。

![](img/52c05b53a56abd53584e4fed738ed509_771_0.png)

如第二个示例所示，`spawnUri` 提供了一种在运行时动态加载和执行代码（甚至可能是整个库）的方法。不要混淆 Futures 和 isolates；它们是不同的，并且应用方式也不同。

- Isolate 用于你希望某些代码真正并行运行的情况，例如一个迷你程序与你的主程序分开运行。你向 isolate 发送消息，并且可以从 isolate 接收消息。每个 isolate 都有自己的事件循环。
- Future 用于你希望在事件循环中稍后某个值可用时得到通知的情况。仅仅要求 Future 运行一个函数并不会使该函数并行运行。它只是将函数安排到事件循环中以便在稍后时间运行。

目前，isolates 在 Erlang 进程的意义上并不非常轻量级，在 Erlang 中，每个进程仅消耗少量内存（数量级为 Kb）。将 isolates 演进到这一理想状态是 Dart 团队的长期目标。此外，isolates 内的异常处理和调试有点粗糙或困难；预计这种情况会改变。isolates 如何映射到操作系统实体（如线程或进程）也没有指定；这可能取决于环境和平台。isolates 尚未扩展到跨 VM 通信。

> 当服务器上运行两个 Dart VM 时，最好使用 TCP sockets 进行通信。你可以启动 ServerSocket 来监听传入请求，并使用 Socket 连接到另一台服务器。

### See also

- 请参阅 *Using isolates in web apps* 配方，了解另一个使用 `spawnUri` 的示例。在 *Using multiple cores with isolates* 配方中可以找到另一个 isolates 的示例。
- 请参阅下一章的 *Using Sockets* 配方，了解有关 `Sockets` 和 `ServerSockets` 的更多信息。
- 请参阅 *Chapter 2, Structuring, Testing, and Deploying an Application* 中的 *Profiling and benchmarking your app* 配方，了解有关基准测试的更多信息。

## Using isolates in web apps

在本配方中，你将学习如何在项目 `using_web_isolates` 的 Web 应用中使用 isolates。此示例在嵌入浏览器的 Dart VM 中运行，以及编译为 JavaScript。在后一种情况下，它使用 HTML5 Web workers，在后台独立于其他脚本运行，而不影响页面的性能。

### How to do it...

`using_web_isolates.dart` 中的主 isolate 运行以下代码：

```dart
import 'dart:isolate';
import 'dart:html';
import 'dart:async';

main() {
  Element output = querySelector('#output');
  SendPort sendPort;
  ReceivePort receivePort = new ReceivePort();
  receivePort.listen((msg) {
    if (sendPort == null) {
      sendPort = msg;
    } else {
      output.text += 'Received from isolate: $msg\n';
    }
  });

  String workerUri = 'worker.dart';
  int counter = 0;
  // start 3 isolates:
  for (int i = 1; i <= 3; i++) {
    Isolate.spawnUri(Uri.parse(workerUri), [], receivePort.sendPort).
      then((isolate) {
        print('isolate spawned');
        new Timer.periodic(const Duration(seconds: 1), (t) {
          sendPort.send('From main app: ${counter++}');
          if (counter == 10) {
            sendPort.send('END');
            t.cancel();
          }
        });
      });
  }
}
```

以下是 isolate worker.dart 的代码：

```dart
import 'dart:isolate';

main(List<String> args, SendPort sendPort) {
  ReceivePort receivePort = new ReceivePort();
  sendPort.send(receivePort.sendPort);

  receivePort.listen((msg) {
    if (msg == 'END') receivePort.close();
    sendPort.send('ECHO: $msg');
  });
}
```

以下是 Web 页面上显示的输出：

![](img/52c05b53a56abd53584e4fed738ed509_774_0.png)

Isolates in a web app

我们看到来自三个 isolates 的消息传入。在计数到 10 时，计时器停止，并且 isolates 收到关闭消息。

### How it works...

主 isolate 首先设置一个 ReceivePort 和对 isolates 的监听器。此监听器首先捕获 isolate 的 sendPort，然后将每条消息附加到 Web 页面上。Web 应用中的 isolate 必须使用 spawnUri 启动；代码位于脚本 worker.dart 中。然后根 isolate 启动一个计时器，并每秒发送已过去的秒数。10 秒后，计时器被取消，并发送 END 消息。

我们启动了三个 isolate，每个都执行相同的代码。isolates 代码中的 main 方法将其第一个参数接收为 List<String> args。这来自 spawnUri 的第二个参数；这是主 isolate 向 isolate 发送初始数据的一种方式，但这里它是空列表。每个 isolate 首先设置其 ReceivePort，并将其 sendPort 发送给主 isolate，以便建立通信线路。然后，它开始监听并回显收到的内容。isolate 在收到 END 消息时退出。使用 dart2js 编译为 JavaScript 时，.js 会自动添加到脚本名称 worker.dart 的末尾。

### See also

- 请参阅 Using isolates in the Dart VM 配方，了解有关 isolates 的一般解释。

## Using multiple cores with isolates

在本配方中，我们向你展示 Dart VM 在无需向 VM 指定任何内容的情况下使用处理器上的多个核心。这比 Dart 应用只能使用一个处理器时能提供更好的性能和吞吐量。

### How to do it...

查看以下项目 using_isolates 中 many_cores.dart 的代码：

```dart
import 'dart:isolate';

main() {
  int counter = 0;
  ReceivePort receivePort = new ReceivePort();
  receivePort.listen((msg) {
    if (msg is SendPort) {
      msg.send(counter++);
    } else {
      print(msg);
    }
  });

  // starting isolates:
  for (var i = 0; i < 5; i++) {
    Isolate.spawn(runInIsolate, receivePort.sendPort);
  }
}

// code to run isolates
runInIsolate(SendPort sendPort) {
  ReceivePort receivePort = new ReceivePort();
  // send own sendPort to main isolate:
  sendPort.send(receivePort.sendPort);

  receivePort.listen((msg) {
    var k = 0;
    var max = (5 - msg) * 50000000;
    for (var i = 0; i < max; ++i) {
      i = ++i - 1;
      k = i;
    }
  }
}
```

