
If the total work saturates the core, then the additional work will strangle and slow down the responsiveness of the server for all the clients as later requests queue up and wait for the previous work to be completed. **Isolates** can solve this problem and run several instances of the server in different threads. We will continue to improve our server and use the `ServerSockets` feature that came with the Dart 1.4 release. We will use the references of `ServerSocket` to run multiple instances of our server simultaneously. Instead of creating an instance of `HttpServer`, we create `ServerSocket` with the same initial parameters that we used before.

First of all, we need to create `ReceivePort` in the main thread to receive hand-shaking and usual messages from the spawned isolates. We create as many isolates as we can depending on the number of processors we have. The first parameter of the spawn static method of the `Isolate` class is a global function that helps organize hand-shaking between the main thread and spawned isolate. The second parameter is port, which is used as a parameter in the global function. The same port is used to send messages from spawned isolates to the main thread. Now, we need to listen to the messages from the spawned isolates. The spawned isolate follows the hand-shaking process and all the sent messages with `SendPort` are listened to in the main thread. On the completion of the hand-shaking procedure, we create and send an instance of `ServerTask`. All other messages will come as a string to be printed out on the console, as shown in the following code:

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

Our code is clear enough and potentially faster with isolates. The program is clearer because the code to handle each request is nicely wrapped up in its own function and is faster because each ServerSocket instance keeps different connections asynchronous and independent; the work on one connection doesn't have to wait to be processed sequentially behind work on another connection. In general, this gives a better responsiveness even on a single-core server. In practice, it delivers better scalability under the load on servers that do have parallel hardware. Now, run the tests and we will see a significant improvement in our server:

```
./wrk -t1 -c256 -d30s http://127.0.0.1:8080
```

The following is the result of the preceding code:

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

Our server can process close to 24,000 requests per second in a concurrency-enabled environment. Fantastic!

So, after that quick dive into the world of concurrency, let's discuss isolates in general. Just repeat the best practices of the async programming:

- The program is driven by the queued events coming in from different independent sources
- All the pieces of the program must be loosely coupled

Isolate is a process that builds around the model of servicing a simple FIFO messaging queue. It does not share memory with other isolates and all isolates communicate by passing messages, which are copied before they are sent. As you can see, the implementation of isolates follows the same main principles as async programming.

> Always set the receiver port to the main isolate if you need to receive messages from other isolates or send them to each other.

## Summary

You now have a better understanding of event-driven architecture, which is one of the key concepts of Dart VM. Event-driven architecture is the right approach to build loosely coupled asynchronous systems.

Dart relies on event-driven architecture based on a single-threaded execution model with a single event loop and two queues. The event loop is backed by a single thread, so no synchronization or locks are required at all. When the event loop is blocked with an operation, this blocks the entire application. A combination of single-threaded execution models and asynchronous operations allows an application to be more productive and less resource intensive.

Future is a proxy for an initially unknown result that returns as a value instead of calling a callback function. Future almost always adds an event or microtask into the queue that is being processed in the event loop. Future can be completed with a value or error only once.

Zones implement the best practices of a configurable code wrapper to handle the uncaught errors. Zones can have local variables and can schedule microtasks, create one-off or repeating timers, print information, and save a stack trace for debugging purposes.

An isolate is a process built around the model of servicing simple messaging queue. It does not share the memory with other isolates and all isolates communicate by passing messages, which are copied before they are sent.

In the next chapter, we will see the stream framework and show when and how to properly use it.

## 5 The Stream Framework

In this chapter, we will talk about streams. Streams have existed since the early days of UNIX. They have proven to be a dependable way to compose large systems out of small components, which does one thing well. Streams restrict the implementation of a surface area into a consistent interface that can be reused. You can plug the output of one stream as the input to another and use libraries that operate abstractly on streams to institute high-level flow control. Streams are an important component of small program design and have important abstractions that are worth considering. In this chapter, we will cover the following topics:

- Single-subscription streams versus broadcast streams
- The stream framework API

### Why you should use streams

Just imagine that you need to provide a file on a server in response to a client's request. The following code will do this:

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
