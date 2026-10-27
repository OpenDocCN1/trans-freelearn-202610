

如果总的工作量使核心饱和，那么额外的任务会扼杀并减慢服务器对所有客户端的响应速度，因为后续请求会排队等待之前的工作完成。**Isolates** 可以解决这个问题，并在不同的线程中运行服务器的多个实例。我们将继续改进我们的服务器，并使用随 Dart 1.4 版本发布的 `ServerSockets` 功能。我们将使用 `ServerSocket` 的引用同时运行多个服务器实例。不是创建 `HttpServer` 的实例，我们使用之前使用的相同初始参数创建 `ServerSocket`。

首先，我们需要在主线程中创建 `ReceivePort`，以接收从已生成的 isolates 发出的握手和常规消息。我们根据拥有的处理器数量创建尽可能多的 isolates。`Isolate` 类的 `spawn` 静态方法的第一个参数是一个全局函数，有助于在主线程和已生成的 isolate 之间组织握手。第二个参数是端口，作为全局函数的参数使用。相同的端口也用于从已生成的 isolates 向主线程发送消息。现在，我们需要监听来自已生成 isolates 的消息。已生成的 isolate 遵循握手过程，并且所有使用 `SendPort` 发送的消息都在主线程中被监听。握手过程完成后，我们创建并发送一个 `ServerTask` 实例。所有其他消息将作为字符串到来，并在控制台上打印，如以下代码所示：

```
import 'dart:isolate';
import 'dart:io';

main() {
  ServerSocket
    .bind(InternetAddress.ANY_IP_V4, 8080)
    .then((ServerSocket server) {
      // Create main ReceivePort
      ReceivePort receivePort = new ReceivePort();
      // Create as much isolates as possible
      for (int i = 0; i < Platform.numberOfProcessors; i++) {
        // Create isolate and run server task
        Isolate.spawn(runTask, receivePort.sendPort);
      }
      // Start listening messages from spawned isolates
      receivePort.listen((msg) {
        // Check what the kind of message we received
        if (msg is SendPort) {
          // There is hand-shaking message.
          // Let's send ServerSocketReference and port
          msg.send(new ServerTask(
              server.reference, receivePort.sendPort));
        } else {
          // Usual string message from spawned isolates
          print(msg);
        }
      });
    });
}

/**
 * Global function helps organize hand-shaking between main
 * and spawned isolate.
 */
void runTask(SendPort sendPort) {
    // Create ReceivePort for spawned isolate
    ReceivePort receivePort = new ReceivePort();
    // Send own sendPort to main isolate as response on hand-shaking
    sendPort.send(receivePort.sendPort);
    // First message comes from main contains a ServerTask instance
    receivePort.listen((ServerTask task) {
        // Just execute our task
        task.execute();
    });
}

/**
 * Task helps create ServerSocket from ServerSocketReference.
 * We use new instance of ServerSocket to create new HttpServer
 * which starts listen HttpRequests and sends requested path into
 * main's ReceivePort.
 */
class ServerTask {
    ServerSocketReference reference;
    SendPort port;

    ServerTask(this.reference, this.port);

    execute() {
        // Create ServerSocket
        reference.create().then((serverSocket) {
            // Create HttpServer and start listening incoming HttpRequests
            new HttpServer.listenOn(serverSocket)
                .listen((HttpRequest request) {
                    // Send requested path into main's ReceivePort
                    port.send(request.uri.path);
                    // Response back to client
                    request.response.write("Hello, world");
                    request.response.close();
                });
        });
    }
}
```

我们的代码足够清晰，并且在 isolates 的帮助下性能潜力更高。程序更清晰，因为处理每个请求的代码被很好地封装在其自己的函数中，并且更快，因为每个 ServerSocket 实例保持不同的连接异步且独立；一个连接上的工作不必在另一个连接上的工作之后按顺序等待处理。总的来说，这即使在单核服务器上也能提供更好的响应性。在实践中，在具有并行硬件的服务器上，它在负载下提供了更好的可扩展性。现在，运行测试，我们将看到服务器的显著改进：

```
./wrk -t1 -c256 -d30s http://127.0.0.1:8080
```

以下是前述代码的结果：

```
Running 30s test @ http://127.0.0.1:8080
  1 threads and 256 connections
Thread Stats   Avg   Stdev   Max   +/- Stdev
  Latency    10.31ms   6.11ms  50.81ms   73.78%
    Req/Sec   24.01k   2.00k  28.32k   67.52%
  709163 requests in 30.00s, 46.67MB read
Requests/sec:  23638.95
Transfer/sec:      1.56MB
```

我们的服务器在支持并发的环境中可以处理接近 24,000 个请求每秒。太棒了！

因此，在快速探索并发世界之后，让我们总体讨论 isolates。只需重复异步编程的最佳实践：

- 程序由来自不同独立源的排队事件驱动
- 程序的所有部分必须松散耦合

Isolate 是一个围绕为简单的 FIFO 消息队列提供服务而构建的进程。它不与其他 isolates 共享内存，并且所有 isolates 通过传递消息进行通信，消息在发送前会被复制。正如你所见，isolates 的实现遵循与异步编程相同的核心原则。

> 如果你需要从其他 isolates 接收消息或相互发送消息，请始终将接收端口设置为主 isolate。

## Summary

你现在对事件驱动架构有了更好的理解，这是 Dart VM 的关键概念之一。事件驱动架构是构建松散耦合的异步系统的正确方法。

Dart 依赖于基于单线程执行模型、单个事件循环和两个队列的事件驱动架构。事件循环由单个线程支持，因此完全不需要同步或锁。当事件循环被操作阻塞时，整个应用程序都会被阻塞。单线程执行模型与异步操作的结合使应用程序更具生产力且资源消耗更少。

Future 是对初始未知结果的代理，其返回值而不是调用回调。Future 几乎总是向正在事件循环中处理的队列添加一个事件或微任务。Future 只能用值或错误完成一次。

Zones 实现了可配置的代码包装器的最佳实践，以处理未捕获的错误。Zones 可以具有局部变量，可以调度微任务、创建一次性或重复计时器、打印信息，并为调试目的保存堆栈跟踪。

Isolate 是一个围绕为简单消息队列提供服务而构建的进程。它不与其他 isolates 共享内存，并且所有 isolates 通过传递消息进行通信，消息在发送前会被复制。

在下一章中，我们将看到流框架并展示何时以及如何正确使用它。

## 5 The Stream Framework

在本章中，我们将讨论流。流自 UNIX 早期就已存在。它们已被证明是将大型系统由执行单一功能良好的小型组件组合起来的可靠方式。流将实现的表面限制为一致的接口，可以重复使用。你可以将一个流的输出作为另一个流的输入，并使用在流上抽象操作的库来实现高级流控制。流是小型程序设计的重要组成部分，并且具有值得考虑的重要抽象。在本章中，我们将涵盖以下主题：

- 单订阅流与广播流
- 流框架 API

### Why you should use streams

假设你需要在响应客户端请求时提供服务器上的一个文件。以下代码将执行此操作：

```
import 'dart:io';

main() {
  HttpServer
    .bind(InternetAddress.ANY_IP_V4, 8080)
    .then((server) {
      server.listen((HttpRequest request) {
        new File('data.txt').readAsString()
          .then((String contents) {
            request.response.write(contents);
            request.response.close();
          });
      });
    });
}
```

