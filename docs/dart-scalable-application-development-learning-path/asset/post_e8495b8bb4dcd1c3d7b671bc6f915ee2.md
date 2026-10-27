
3. To handle the response, use the following code:

```
requestComplete(HttpRequest request) {
  if (request.status == 200 && request.readyState == HttpRequest.DONE) {
    Blob blob = request.response;
    flr.onLoadEnd.listen( (e) {
      img.src = flr.result;
    });
    flr.readAsDataUrl(blob);
  }
  else {
    print('Request failed, status=${request.status}');
  }
}
```

Try it out with the 5 MB file stadium.jpg.

## How it works...

To read the blob, we will need a FileReader object; we constructed this in step 1. In step 2, we build the request object, which is fairly general, except that we set responseType and mimeType. In the callback function in step 3, we test with request.readyState == HttpRequest.DONE to be sure that the request has been fully handled. Then, we performed the following steps:

- We created a Blob object from the Blob class in dart:html and set it to the response
- We read the blob with the method readAsDataUrl
- When this was completed (signaled by the onLoadEnd event), the source of the image tag was set to the result of the FileReader object

## Transforming streams

Listening to a stream captures the sequence of results coming from an event-like action, such as clicking on a button in a web page or opening a file with the openRead method. These results are data that can be processed, but the errors that occur are also part of the stream. Dart can work with streams in a very functional way, such as filtering the results with where or mapping the results to a new stream (for a complete list of these methods, refer to https://api.dartlang.org/apidocs/channels/stable/dartdoc-viewer/dart:async.Stream). To modify the incoming results, we can also use a transformer; this recipe shows you how to do this (refer to the project transforming_stream).

## How to do it...

In our script, we have a list, persons, where the items are themselves lists consisting of a name and a gender. We want to walk through the list and emit a greeting message based on the gender of the person, but if the gender is unknown, we skip that person. The following code shows us how we can do this with a transformer:

```
```dart
import 'dart:async';

var persons = [
  ['Carter', 'F'],
  ['Gates', 'M'],
  ['Nuryev', 'M'],
  ['Liszt', 'U'],
  ['Besançon', 'F']
];

void main() {
```
```

We need to perform the following steps to transform the streams:

- 1. To make a stream from the list, use the following code:
  
      ```dart
      var stream = new Stream.fromIterable(persons);
      ```
  
- 2. To define a stream transformer, use the following code:
  
      ```dart
      var transformer = new
          StreamTransformer.fromHandlers(handleData: convert);
      ```
  
- 3. To filter and transform the stream, and listen to its output to process further, use the following code:
  
      ```dart
      stream
        .where((value) => value[1] != 'U')
        .transform(transformer)
        .listen((value) => print("$value"));
      }
      ```

```
```dart
convert(value, sink) {
  // Create a new value from the original value
  var greeting = "Hello Mr. or Mrs. ${value[0]}";
  if (value[1] == 'F') {
    greeting = "Hello Mrs. ${value[0]}";
  }
}
```
```

```
else if (value[1] == 'M') {
    greeting = "Hello Mr. ${value[0]}";
}
    sink.add(greeting);
}
```

After performing the preceding steps, we get the following output:

Hello Mrs. Carter

Hello Mr. Gates

Hello Mr. Nureyev

Hello Mrs. Besançon

## How it works...

To turn a list into a stream, we used the fromIterable method as in step 1. Discarding some values from the stream can be done with where; see the first clause in step 3.

Step 2 details how to transform a stream. This method takes an object (here called transformer) of the class StreamTransformer, which allows you to change the contents of the stream. The constructor named fromHandlers takes an optional handleData argument that calls our callback function convert for each value passed from the stream. The convert option builds a new value based on the content of the old value and adds it in place of the old value of the sink variable. Only those transformed values are output on the stream, passed on to listen, and processed there. The sink option is an instance of the abstract class StreamSink, which is a generic destination of data and can be implemented by any data receiver.

## There's more...

We have already used transform in this chapter when reading a file with openRead, as shown in the following code:

```
Stream<List<int>> input = file.openRead();
input
.transform(UTF8.decoder)
.transform(const LineSplitter())
```

## Creating a web server

The class `HttpServer` is used to write web servers; this server listens on (or binds to) a particular host and port for incoming HTTP requests. It provides event handlers (better called `request` handlers in this case) that are triggered when a request with incoming data from a web client is received.

## How to do it...

We make a project called `simple_webserver` starting from the template command-line application and import `dart:io` as follows:

```
import 'dart:io';
//Define host and port:
InternetAddress HOST = InternetAddress.LOOPBACK_IP_V6;
const int PORT = 8080;

main() {
  // Starting the web server:
  HttpServer.bind(HOST, PORT)
    .then((server) {
      print('server starts listening on port ${server.port}');
      // Starting the request handler:
      server.listen(handleRequest);
    })
    .catchError(print);
}

handleRequest(HttpRequest req) {
  print('request coming in');
  req.response
    ..headers.contentType = new ContentType("text", "plain",
      charset: "utf-8")
    ..write(' I heard you loud and clear.')
    ..write(' Send me the data!')
    ..close();
}
```

## Working with Web Servers

This produces the following server console output:

![](img/52c05b53a56abd53584e4fed738ed509_726_0.png)

Console output from the web server

Then, start any browser with the URL `http://localhost:8080` to see the response text appear on the client as shown in the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_726_1.png)

The browser client shows the response

## There is more...

The `HttpRequest` object also has properties that provide information about the client's request; the most important ones are as follows:

- `method`: This is derived from the way the web form was submitted. In the `<form action="http://localhost:4041" method="GET">` code, the values it can take are **GET, POST, PUT,** or **DELETE**.
- `headers`: This gives general information on the request, such as content type, content length, and date.
- `uri`: This gives the location where the request originated from.

## Posting JSON-formatted data

This is a recipe for a client web app that sends a request to a web server. The request contains the form's data that is posted in the JSON format.

## How to do it...

Look at the project post_form for the code.

- 1. Our form (refer the next diagram) will post data for a job in IT; we reuse the class Job in the Making toJSON and fromJSON methods in your class recipe from Chapter 4, Object Orientation. We keep the example short and simple, but add two new properties, posted and open. Have a look at the following code:
  ```
  class Job {
      String type;
      int salary;
      String company;
      DateTime posted; // date of publication of job
      bool open = true; // is job still vacant ?
      Job(this.type, this.salary, this.company, this.posted);
      // toJSON and fromJSON methods
  }
  ```
- 2. The model class is made available to the code in post_form.dart using the following code:
  ```
  import '../model/job.dart';
  ```
- 3. We add our own event handler for the submit button using the following code:
  ```
  void main() {
      querySelector("#submit").onClick.listen(submitForm);
  }
  ```
- 4. The method submitForm makes and sends the request as follows:
  ```
  submitForm(e) {
      e.preventDefault(); // Don't do the default submit.
      // send data to web server:
      req = new HttpRequest();
      req.onReadyStateChange.listen(onResponse);
      // POST the data to the server.
      var url = 'http://127.0.0.1:PORT';
      req.open('POST', url);
      req.send(_jobData()); // send JSON String to server
  }
  ```
