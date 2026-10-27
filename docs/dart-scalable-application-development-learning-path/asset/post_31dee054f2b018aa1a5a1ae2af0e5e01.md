
```dart
'${socket.remoteAddress.address}:${socket.remotePort}');
socket.destroy();
});
// prints: Connected to: 173.194.65.101:80

Socket.connect(HOST, PORT).then((socket) {
  print(socket.runtimeType);
  // data to server:
  socket.write('Hello, World from a client!');
  // data from server:
  socket.listen(onData);
});
}

onData(List<int> data) {
  print(new String.fromCharCodes(data));
}
```

- 3. Start one (or more) client(s) by running `bin\socket_client.dart`.

The following is the output from the server console:

**serversocket is ready**

**Connection from: ::200:0:8017:7b01%211558873:6564**

**Hello, World from a client!**

The following is the output from a client console:

**_Socket**

**Hello from Simple Socket Server!**

**Connected to: 74.125.136.139:80**

We can see that there is two-way communication.

### How it works...

In step 1, a server to handle client socket connections is created by binding `ServerSocket` to a specific TCP port that it will listen on. This bind method returns a `Future<ServerSocket>`. Again, we will use the `Future.then` method to register our callback so that we know when the socket has been bound to the port. Then, the server starts listening and calls `handleClient` for each incoming connection. This callback prints the remote address from the client, prints the data the client has sent, writes a message to the client, and then closes the connection.

In step 2, the client first opens a connection to www.google.com on port 80 (the port that serves web pages). After the socket is connected to the server, the IP address and port are printed to the screen. Then, the socket is shut down using socket.destroy. In the second part of this step, we connect to the local socket server, write a message to it with socket.write, and start listening to the server with socket.listen. We transform the data that comes in as a list of integers into a string that is printed out.

### There's more...

Socket communication is often blocked by firewalls; if this is an issue, take a look at WebSockets in the following recipe.

Dart also supports UDP socket programming; the article by James Slocum offers a detailed discussion at http://jamesslocum.com/post/77759061182.

## Using WebSockets

This recipe will show you how to use WebSockets in a client-server application (both web and command-line clients) and what its advantages are. You can find the code in the project websockets.

### Getting ready

HTTP is a simple request-response-based protocol, and then the connection is broken from the application's point of view until the next request. In a modern web application (for example, in online multiplayer games), the client and server are of equal importance; changes in the state of the application can take place on both sides. So, we need a bi-directional communication channel between the client(s) and the server that allows for two-way real time updates and more interaction; this is exactly what WebSockets has to offer. WebSocket connections between a browser and a server are made through a handshake request. This is a regular HTTP client request with an upgrade flag in the header, also containing a Sec-WebSocket-Key, which is a random value that has been base64 encoded. To complete the handshake, the server responds with a Sec-WebSocket-Accept response.

WebSockets are implemented in all modern browsers (Internet Explorer v10 and above), and can also be used in non-web applications. The communication takes place over TCP port 80, so it can pass unhindered through firewalls.

### How to do it...

Perform the following steps to make a WebSockets server work:

- 1. Use the following code to set up a WebSockets server (see `websocket_server.dart`):

```dart
import 'dart:io';
import 'dart:async';

InternetAddress HOST = InternetAddress.ANY_IP_V6;
const PORT = 8080;

main() {
  runZoned(startWSServer, onError: handleError);
}

startWSServer() {
  HttpServer.bind(HOST, PORT)
    .then((server) {
      print('Http server started on $HOST $PORT');
      server.listen(handleRequest);
    });
}

handleError(e, stackTrace) {
  print('An error occurred: $e $stackTrace');
}

handleRequest(HttpRequest req) {
  if ((req.uri.path == '/ws') // command-line client
      || WebSocketTransformer.isUpgradeRequest(req) // web-client
     ) {
    // Upgrade a HttpRequest to a WebSocket connection.
    WebSocketTransformer.upgrade(req).then(handleWebSocket);
  }
  else {
    print("Regular ${req.method} request for: ${req.uri.path}");
    serveNonWSRequest(req);
  }
}

handleWebSocket(WebSocket socket) {
  print('Client connected!');
  socket.listen((String msg) {
    print('Message received: $msg');
});

ws.onError.first.then((_) {
    print("Failed to connect to ${ws.url}. "
    "Please rerun bin/websocket_server.dart and try again.");
});
}

void outputMessage(Element e, String message){
    print(message);
    e.appendText(message);
    e.appendHtml('<br/>');
    //Make sure we 'autoscroll' the new messages
    e.scrollTop = e.scrollHeight;
}
```

When the server runs, start the web client by opening websocket_webclient.html, and type in some text. The text received by the web server is echoed back and shown both on the page and in the editor console as follows:

![](img/52c05b53a56abd53584e4fed738ed509_744_0.png)

A WebSocket web client

### How it works...

In step 1, the server is run in a runZoned() clause to add additional exception handling capabilities (see the Serving files with http_server recipe). We start a web server as usual. In handleRequest, we check whether the path of the request ends in /ws. In that case, we have a command-line client issuing a WebSocket request. A web client making a WebSocket request will add an upgrade flag in the headers. For this, we can test it with the isUpgradeRequest method of the WebSocketTransformer class. If that is the case, we call the upgrade method on the same class, and when done, we call the handleWebSocket method. This starts listening for client connections, prints out any client message in the server console, and echoes this back to the client. If the message was a JSON string, we could have decoded it before it starts listening to client connections with socket.map((string)=> JSON.decode(string)).

In the case of a normal HTTP request, serveNonWSRequest is used to block it, but of course, we could do normal web request handling as well.

The command-line client in step 2 uses the WebSocket class from dart:io. It connects to a WebSocket server with a ws:// URL as a parameter to the connect method. Then, it can write to the socket with add and receive messages on the socket with listen.

The web client in step 3 uses the WebSocket class from dart:html. Calling its constructor with the URI of the server opens the web socket connection. The send method called on this instance sends the client data (here, the text of the input field) to the server. When the response from the server can be read from the socket, the onMessage event is fired and shows the response. Other useful events of the WebSocket instance are:

- onOpen: This is called when the connection is made
- onClose: This is called when the socket is no longer available (because the server was shut down or a network connection failure)
- onError: This is called when an error occurs during the client-server dialog

### There's more...

The pub package route can be used to associate callbacks with URL patterns. In this recipe, instead of testing the /ws pattern, we could have used Router from the package route to do that for us. We import this package, and then startWSServer will contain the following code:

```dart
import 'package:route/server.dart' show Router;

startWSServer() {
  HttpServer.bind(HOST, PORT).then((server) {
  print('Http server started on $HOST $PORT');
  Router router = new Router(server);
```
