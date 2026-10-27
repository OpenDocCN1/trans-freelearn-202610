# Client-to-server Communication

The SSE code implementation at the client side is as follows:

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

In the preceding code, we created an instance of EventSource with the specified URL. From now, we will start listening to the events from the server. We add the open and error event listeners. The EventSource class informs the code about the closed connection from the server side via changes in the readyState property. The message listener handles all the unknown events. A special event listener for the userlogon event handles the sort of events that could be added via the addEventListener method of the EventSource class. The event keeps the message information in the data property. An event identifier assigned on the server side is available via the lastEventId property, as shown in the following code:

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