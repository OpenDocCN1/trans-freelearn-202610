> When two Dart VMs are running on the server, it is best to use TCP sockets for communication. You can start ServerSocket to listen for incoming requests and use Socket to connect to the other server.

# See also

+   - Refer to the *Using isolates in web apps* recipe for another example using `spawnUri`. Find another example of isolates in the *Using multiple cores with isolates* recipe.
+   - Refer to the *Using Sockets* recipe for more information on `Sockets` and `ServerSockets` in the next chapter.
+   - Refer to the *Profiling and benchmarking your app* recipe in *Chapter 2, Structuring, Testing, and Deploying an Application*, for more information on benchmarking.

# Using isolates in web apps

In this recipe, you will learn how to use isolates in a web application in the project `using_web_isolates`. This example runs in the Dart VM embedded in a browser as well as compiled to JavaScript. In the latter case, it uses HTML5 Web workers, which runs in the background independently of other scripts without affecting the performance of the page.

# How to do it...

The main isolate in `using_web_isolates.dart` runs the following code:

```
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
```