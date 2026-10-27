
In the preceding code, we read the entire file and buffered it into the memory of every request before sending the result to the clients. This code works perfectly for small files, but what will happen if the data.txt file is very large? The program will consume a lot of memory because it serves a lot of users concurrently, especially on slow connections. One big disadvantage of this code is that we have to wait for an entire file to be buffered in memory before the content can be submitted to the clients.

The HttpServer object listens for the HTTP request, which is a Stream. The HttpServer object then generates an HttpRequest object and adds it to the stream. The body of the request that is delivered by an HttpRequest object is a stream of byte lists. An HttpRequest object provides you with an access to the response property associated with an HttpResponse object. We will write the content of a file into the body of the HttpResponse object. The fact that the HttpRequest and HttpResponse classes are streams means that we can write our example in a better way, as shown in the following code:

```
import 'dart:io';

main() {
  HttpServer
    .bind(InternetAddress.ANY_IP_V4, 8080)
    .then((server) {
      server.listen((HttpRequest request) {
        new File('data.txt')
          .openRead()
          .pipe(request.response);
      });
    });
}
```

The `openRead` method creates a new independent `Stream` instance of byte lists for the content of this file. The `pipe` method binds this stream as the input of the provided stream consumer and listens for the data, error, or done events. The preceding code has the following benefits:

- The code is cleaner, so you don't need to remember how to push data through the nonstreaming API
- The file streams to clients in chunks, one chunk at a time as soon as they are received from the disk
- The server doesn't need to buffer chunks of a file in the memory when the remote clients are on a slow or high-latency connection, because the stream handling backs the pressure automatically

Streams make development simple and elegant.

## Single-subscription streams versus broadcast streams

The Dart stream framework supports the single-subscription and broadcast streams in order to have different approaches depending on the required solution. Let's see the difference between single-subscription and broadcast streams.

### A single-subscription stream

A stream that allows you to have only one listener during the entire lifetime is called a single-subscription stream. You are not allowed to cancel and subscribe to the same stream again. The newly created stream starts generating events only after the subscription starts listening to them. It stops generating events after the subscription is canceled even if the stream can provide more. The single-subscription stream always delivers each event in the correct order to a listener.

> Use single-subscription streams when the event delivery and its order are important for your solution.

A good usage example of a single-subscription stream is getting data from the file or server.

### A broadcast stream

A stream that allows you to have multiple listeners during the entire lifetime is called the broadcast stream. You are allowed to subscribe or cancel the subscription at any time. A broadcast stream starts to generate events immediately after the creation if it is ready, independent of whether any subscription is registered or not. This fact means that some portion of the events can be lost in a moment when no listener is registered. There is no guarantee that multiple listeners will get the event in the same order that they were registered. A broadcast stream only guarantees that each listener will get all the events in the correct order.

> Use broadcast streams when delivering events to multiple listeners is important for your solution.

A good example of using the broadcast stream is the eventbus implementation that supports multiple listeners.

The execution of the preceding code prints the following result on the console:

```
2
done
```

## A new stream from the Iterable class

It's quite obvious that you should have a factory constructor that creates a single-subscription stream from any Iterable instance. Iteration over a stream happens only if the stream has a listener and can be interrupted if the listener cancels the subscription. Any error that occurs in the iteration process immediately ends the stream and throws an error. In this case, the done event doesn't fire because the iteration was not complete.

In the following code snippet, we create an instance of the Iterable class with a factory constructor that generates the sequence of numbers. We intentionally generate an Exception at the location of the third element:

```
import 'dart:async';

main() {
  // some sample generated data
  var data = new Iterable<num>.generate(5, (int indx) {
    if (indx < 3) {
      return indx;
    } else {
      throw new Exception('Wrong data');
    }
  });
  // create the stream
  Stream<num> stream = new Stream<num>.fromIterable(data);
  // Start listening
  var subscriber = stream.listen((data) {
    print(data);
  }, onError: (error) {
    print(error);
  }, onDone: () {
    print('done');
  });
}
```

The result of the preceding code is as follows:

```
0
1
2
Exception: Wrong data
```

## The Stream Framework

### A new stream with periodically generated events

In the previous topic, we used the `Iterable.generate` factory constructor to emit numeric data into our stream. However, we can do that as well via the `Stream.periodic` factory constructor, as follows:

```
factory Stream.periodic(Duration period,
    [T computation(int computationCount)])
```

We can do this in a more natural and less verbose way, as follows:

```
import 'dart:async';

main() {
  // some sample generated data
  Stream<num> stream = new Stream
    .periodic(const Duration(milliseconds: 500), (int count) {
      // Return count
      return count;
    });
  // Start listening
  var subscriber = stream.listen((data) {
    print(data);
  }, onError: (error) {
    print(error);
  }, onDone: () {
    print('done');
  });
}
```

The first parameter is the duration that gives you the interval between emitting events. The second parameter is the callback function that computes the event values. This function has a single parameter that defines the sequential number of iterations.

### A new stream from the transformation pipe

The `Stream.eventTransformed` factory constructor is quite interesting because it creates a new `Stream` from the existing one with the help of a sink transformation, as shown in the following code:

```
factory Stream.eventTransformed(Stream source,
    EventSink mapSink(EventSink<T> sink))
```

The first parameter is the source stream that provides events to the new one. The mapSink callback function is the one that is called when a new stream is listening. All the events from the existing stream pass through the sink to reach a new stream. This constructor is widely used to create stream transformers. In the following code, we will create a DoublingSink class. It accepts the output stream as an argument of the constructor. We will implement the number-doubling algorithm inside the add method. The other addError and close methods are simple and pass the incoming parameter values to the underlying stream object, as shown in the following code:

```
import 'dart:async';

/**
 * An interface that abstracts creation or handling of
 * Stream events.
 */
class DoublingSink implements EventSink<num> {
  final EventSink<num> _output;

  DoublingSink(this._output);

  /** Send a data event to a stream. */
  void add(num event) {
    _output.add(event * 2);
  }

  /** Send an async error to a stream. */
  void addError(errorEvent, [StackTrace stackTrace]) =>
      _output.addError(errorEvent, stackTrace);

  /** Send a done event to a stream.*/
  void close() => _output.close();
}
```

The DoublingTransformer class implements StreamTransformer for numbers. In the bind method, which is compulsory, we will create a new stream via the `eventTransformed` constructor and return the instance of DoublingSink as result of the constructor's callback, as shown in the following code:

```
class DoublingTransformer implements StreamTransformer<num, num> {
  Stream<num> bind(Stream<num> stream) {
    return new Stream<num>.eventTransformed(stream,
      (EventSink sink) => new DoublingSink(sink));
  }
}
```
