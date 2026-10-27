The EventSource class is used to receive SSE. This class connects via a specified URL to a server over HTTP and receives server events without closing the connection. The events come in a text/event-stream format. To open a connection to a server and start receiving events, we create a new instance of the EventSource class via a factory constructor and pass the URL of the resource that generates the events through it. Once the connection is established, we wait for the messages.

> The URL of the resource that generates the events must match the origin of the calling page.

On the server side, each message is sent as a block of text terminated by a pair of new lines. The text data is encoded with UTF-8. Each message consists of one or more lines of text listing the fields for that message. Each field is represented by the field name, followed by a colon and the text data for that field's value. Here is an example of data-only messages:

```
; this is a comment
data: some text

data: another text
```

In the preceding code, the first line is just a comment. All the text messages starting with a colon character are comments. A comment could be useful as Keep-Alive if the messages are not sent regularly. The second and the third line contains just a data field with a text value. The third line contains an extra new line character terminating a message. Several events could be sent via a message. Each event has a name specified in the event field and data field whose values are any string. Data could also be in a JSON format as shown in the following code:

```
event: userLogon
data: {"username": "John", "time": "01:22:45", "text": "Hello World"}

event: userMessage
data: "Any data"
```

You can use unnamed events in messages without the event field. The SSE implementation uses message as a name for unnamed events. Each event might have an id. In this scenario, data can be combined as follows:

```
id: 123
data: some text
data: {"text": "Another text}
```