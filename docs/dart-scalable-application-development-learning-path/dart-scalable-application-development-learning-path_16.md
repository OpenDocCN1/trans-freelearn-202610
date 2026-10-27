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





在前面的代码中，我们在向客户端发送结果之前，先读取整个文件并将其缓冲到每个请求的内存中。这段代码对于小文件来说运行完美，但如果 data.txt 文件非常大，会发生什么？程序会消耗大量内存，因为它同时为大量用户提供服务，尤其是在连接缓慢的情况下。这段代码的一个大缺点是，我们必须等待整个文件缓冲到内存中后，才能将内容提交给客户端。

HttpServer 对象监听 HTTP 请求，该请求是一个 Stream。HttpServer 对象随后生成一个 HttpRequest 对象并将其添加到流中。由 HttpRequest 对象提供的请求体是一个字节列表的流。HttpRequest 对象为您提供了访问与 HttpResponse 对象关联的 response 属性。我们将把文件的内容写入 HttpResponse 对象的 body 中。HttpRequest 和 HttpResponse 类都是流这一事实意味着我们可以以更好的方式编写示例，如以下代码所示：

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

`openRead` 方法为此文件的内容创建一个新的独立的字节列表 `Stream` 实例。`pipe` 方法将此流绑定为提供的流消费者的输入，并监听数据、错误或完成事件。前面的代码具有以下优点：

- 代码更简洁，因此您无需记住如何通过非流式 API 推送数据
- 文件以块的形式流式传输到客户端，一次一块，只要从磁盘接收到就立即传输
- 当远程客户端处于缓慢或高延迟连接时，服务器无需将文件的块缓冲在内存中，因为流处理会自动进行背压

流使开发变得简单而优雅。

## 单订阅流与广播流

Dart 流框架支持单订阅和广播流，以便根据所需解决方案采用不同的方法。让我们看看单订阅流和广播流之间的区别。

### 单订阅流

允许您在整个生命周期内只有一个监听器的流称为单订阅流。您不允许取消并再次订阅同一流。新建的流仅在订阅开始监听后才开始生成事件。在订阅被取消后，即使流仍可提供更多数据，它也会停止生成事件。单订阅流始终以正确的顺序向监听器传递每个事件。

> 当事件的传递及其顺序对您的解决方案很重要时，使用单订阅流。

单订阅流的一个良好使用示例是从文件或服务器获取数据。

### 广播流

允许您在整个生命周期内拥有多个监听器的流称为广播流。您可以在任何时候订阅或取消订阅。广播流在创建后如果已准备好，就会立即开始生成事件，而不论是否已注册任何订阅。此事实意味着在没有监听器注册的瞬间，部分事件可能会丢失。不能保证多个监听器会以相同的顺序接收事件，即它们注册的顺序。广播流仅保证每个监听器都会以正确的顺序接收所有事件。

> 当向多个监听器传递事件对您的解决方案很重要时，使用广播流。

使用广播流的一个良好示例是支持多个监听器的事件总线实现。

前述代码的执行在控制台上打印以下结果：

```
2
done
```

## 从 Iterable 类创建新流

很明显，您应该有一个工厂构造函数，可以从任何 Iterable 实例创建单订阅流。仅当流有监听器时才会发生对流的迭代，如果监听器取消订阅，则可以中断迭代。迭代过程中发生的任何错误都会立即结束流并抛出错误。在这种情况下，done 事件不会触发，因为迭代未完成。

在下面的代码片段中，我们使用工厂构造函数创建 Iterable 类的实例，该构造函数生成数字序列。我们故意在第三个元素的位置生成一个 Exception：

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

前述代码的结果如下：

```
0
1
2
Exception: Wrong data
```

## 流框架

### 使用周期性生成事件的新流

在前一主题中，我们使用 `Iterable.generate` 工厂构造函数将数字数据发送到我们的流中。然而，我们也可以通过 `Stream.periodic` 工厂构造函数来做到这一点，如下所示：

```
factory Stream.periodic(Duration period,
    [T computation(int computationCount)])
```

我们可以以更自然、更简洁的方式做到这一点，如下所示：

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

第一个参数是持续时间，它为您提供事件之间的间隔。第二个参数是计算事件值的回调函数。此函数有一个参数，定义迭代的顺序编号。

### 从转换管道创建新流

`Stream.eventTransformed` 工厂构造函数很有趣，因为它借助 sink 转换从现有流中创建一个新的 `Stream`，如下面的代码所示：

```
factory Stream.eventTransformed(Stream source,
    EventSink mapSink(EventSink<T> sink))
```

第一个参数是源流，它为新流提供事件。mapSink 回调函数是在新流开始监听时调用的。现有流中的所有事件都通过 sink 传递以到达新流。此构造函数广泛用于创建流转换器。在下面的代码中，我们将创建一个 DoublingSink 类。它接受输出流作为构造函数的参数。我们将在 add 方法中实现数字加倍算法。其他 addError 和 close 方法很简单，并将传入的参数值传递给底层流对象，如下面的代码所示：

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

DoublingTransformer 类实现了数字的 StreamTransformer。在 bind 方法中，这是强制性的，我们将通过 `eventTransformed` 构造函数创建一个新流，并将 DoublingSink 的实例作为构造函数回调的结果返回，如下面的代码所示：

```
class DoublingTransformer implements StreamTransformer<num, num> {
  Stream<num> bind(Stream<num> stream) {
    return new Stream<num>.eventTransformed(stream,
      (EventSink sink) => new DoublingSink(sink));
  }
}
```



```dart
}

void main() {
    // some sample data
    var data = [1,2,3,4,5];
    // create the stream
    var stream = new Stream<num>.fromIterable(data);
    // Create DoublingTransformer
    var streamTransformer = new DoublingTransformer();
    // Bound streams
    var boundStream = stream.transform(streamTransformer);
    // Because we start listening the 'bound' stream the 'listen' method
    // invokes the 'doublingTransformer' closure
    boundStream.listen((data) {
        print('$data');
    });
}
```

在 main 方法中，我们通过 `Stream.fromIterable` 工厂构造函数创建了一个简单的流，并创建了 `DoublingTransformer` 的实例作为流转换器。这样，我们可以在 `transform` 方法的调用中将它们组合在一起。当我们开始监听绑定流时，来自源流的事件将在 `DoublingSink` 内部被倍增，并在这里接收。预期结果如下：

```
2
4
6
8
10
```

## 从 StreamController 创建新流

在前面的主题中，我们了解了如何借助某个工厂构造函数轻松地从另一个流创建流。然而，你也可以借助 `StreamController` 从头创建流，这使你可以更好地控制流事件的生成。使用 `StreamController`，我们可以创建一个流，并直接将数据、错误和完成事件发送到该流中。可以通过 `StreamController` 类的不同工厂构造函数来创建流。如果你打算创建单订阅流，请使用以下工厂构造函数：

```dart
factory StreamController({void onListen(), void onPause(), void onResume(), onCancel(), bool sync: false})
```

该控制器具有一个生命周期，包含以下状态：

- **初始状态：** 此时控制器没有订阅。在此状态下，控制器会缓冲所有数据事件。
- **已订阅状态：** 在此状态下，控制器有一个订阅。当订阅者注册或相应地结束订阅时，会调用 `onListen` 和 `onCancel` 回调函数。当订阅者通过控制流将状态更改为暂停或恢复时，会调用回调函数 `onPause` 和 `onResume`。如果来自流的新数据被取消，控制器可能不会调用 `onResume` 回调函数。
- **已取消状态：** 此时控制器没有订阅。
- **已关闭状态：** 在此状态下，不允许再添加事件。

如果 `sync` 属性等于 `true`，则告诉控制器，当调用 `add`、`addError` 或 `close` 方法时，事件可能会被直接传递给正在监听的流。在这种情况下，事件只有在创建事件的代码返回后才会被传递。

控制器会缓冲数据，直到订阅者开始监听，但请记住，这种缓冲方式并未针对大量事件进行优化。以下代码片段展示了如何使用 `StreamController` 创建单订阅流：

```dart
import 'dart:async';

main() {
    // create the stream
    Stream<num> stream = createStream();
    // Start listening
    StreamSubscription<num> sub = createSubscription(stream);
}

StreamSubscription<num> createSubscription(Stream<num> stream) {
    StreamSubscription subscriber;
    subscriber = stream.listen((num data) {
        print('onData: $data');
        // Pause subscription on 3-th element
        if (data == 3) {
            subscriber.pause(new Future.delayed(
                const Duration(milliseconds: 500), () => 'ok'));
        }
    },
    onError: (error) => print('onError: $error'),
    onDone: () => print('onDone'));
    return subscriber;
}

Stream<num> createStream() {
    StreamController<num> controller = new StreamController<num>(
        onListen: () => print('Listening'),
        onPause: () => print('Paused'),
        onResume: () => print('Resumed'),
        onCancel: () => print('Canceled'),
        sync: false);
    //
    num i = 0;
    Future.doWhile(() {
        controller.add(i++);
        // Throws exception on 5-th element
        if (i == 5) {
            controller.addError('on ${i}-th element');
        }
        // Stop stream at 7-th event
        if (i == 7) {
            controller.close();
            return false;
        }
        return true;
    });
    return controller.stream;
}
```

在以上代码中，我们有意在第 5 个元素处抛出错误，并在第 7 个元素处停止流。流订阅者在第 3 个元素处暂停监听流，并在延迟 500 毫秒后恢复。这将产生以下结果：

```
Listening
onData: 0
onData: 1
onData: 2
onData: 3
Paused
onData: 4
onError: on 5-th element
onData: 5
onData: 6
Canceled
onDone
```

以下工厂构造函数为广播流创建控制器，该流可以被多次监听：

```dart
factory StreamController.broadcast({void onListen(), void onCancel(), bool sync: false})
```

当调用 `add`、`addError` 或 `close` 方法时，此构造函数创建的控制器会将数据、错误或完成事件传递给所有监听者。这些同名方法的调用会按顺序发生，并且总是在前一次调用返回之前。与单订阅控制器不同，此控制器没有内部事件队列。这意味着，如果在添加事件时没有注册任何监听者，数据或错误事件将会丢失。每个监听者订阅都独立运作。如果一个订阅暂停，则只有该订阅受到影响，因此所有事件都会在控制器内部缓冲，直到订阅恢复或取消。该控制器的生命周期包含以下状态：

- **初始状态：** 此时控制器没有订阅。在此状态下，控制器会丢失所有已触发的数据和错误事件。
- **已订阅状态：** 此时第一个订阅被添加到控制器。在第一个订阅者注册或最后一个订阅者结束其订阅的那一刻，会调用 `onListen` 和 `onCancel` 回调函数。
- **已取消状态：** 此时控制器没有订阅。
- **已关闭状态：** 在此状态下，不允许再添加事件。

如果 `sync` 属性等于 `true`，则告诉控制器，当调用 `add`、`addError` 或 `close` 方法时，事件可能会被直接传递给正在监听的流。因此，事件将在创建事件的代码返回后被传递，但当多个监听者收到事件时，这一点并不能得到保证。无论 `sync` 属性的值如何，每个监听者都会按正确顺序收到所有事件。以下是前面代码的略微修改版本，包含两个订阅：

```dart
import 'dart:async';

main() {
    // create the stream
    Stream<num> stream = createStream();
    StreamSubscription<num> sub1 = createSubscription(stream, 1);
    StreamSubscription<num> sub2 = createSubscription(stream, 2);
}

StreamSubscription<num> createSubscription(Stream<num> stream, num number) {
    // Start listening
    StreamSubscription subscriber;
    subscriber = stream.listen((num data) {
        print('onData ${number}: $data');
        // Pause subscription on 3-th element
        if (data == 3) {
            subscriber.pause(new Future.delayed(
                const Duration(milliseconds: 500), () => 'ok'));
        }
    },
    onError: (error) => print('onError: $error'),
    onDone: () => print('onDone'));
    return subscriber;
}

Stream<num> createStream() {
    StreamController<num> controller = new StreamController<num>.broadcast(
        onListen: () => print('Listening'),
        onCancel: () => print('Canceled'),
        sync: false);
    //
    num i = 0;
    Future.doWhile(() {
        controller.add(i++);
        // Throws exception on 5-th element
        if (i == 5) {
            controller.addError('on ${i}-th element');
        }
        // Stop stream at 7-th event
        if (i == 7) {
            controller.close();
            return false;
        }
        return true;
    });
    return controller.stream;
}
```



前面的代码片段生成了以下结果：

```
Listening
onData 1: 1
onData 2: 1
onData 1: 2
onData 2: 2
onData 1: 3
onData 2: 3
onData 1: 4
onError: on 5-th element
onData 1: 5
onData 1: 6
onDone
onData 2: 4
onError: on 5-th element
onData 2: 5
onData 2: 6
Canceled
onDone
```

这些结果再次证实了广播流并不能保证向不同监听者的投递事件的顺序。它仅保证每个监听者内部的投递事件顺序。

## StreamSubscription 类的作用是什么？

Stream 类的 listen 方法向流添加以下订阅：

```
StreamSubscription<T> listen(
  void onData(T event),
  { Function onError, void onDone(), bool cancelOnError});
```

每当有新的数据事件从该流发出时，都会调用 onData 回调函数。存在此函数很重要，因为如果没有它就不会发生任何事情。可选的 onError 回调在流发出错误时被调用。此函数接受一个或两个参数。第一个参数始终是来自流的错误。第二个参数如果存在，则是 StackTrace 实例。如果流在没有 StackTrace 的情况下收到错误，则它可以等于 null。当流关闭时，它会调用 onDone 回调函数。cancelOnError 标志通知订阅在错误发生的那一刻开始取消。

## 流框架

此方法的结果是 `StreamSubscription` 类的实例。它向监听者提供事件并保存 listen 方法中 `Stream` 类使用的回调。你可以通过 `StreamSubscriber` 类的 `onData`、`onError` 和 `onDone` 方法设置或覆盖全部三个回调函数。监听的流可以用 pause 和 resume 方法暂停和恢复。特殊的标志 `isPaused` 在 `StreamSubscription` 类的实例被暂停时返回 true。流订阅可以用 cancel 方法随时结束。它返回一个 `Future` 实例，当流完成清理时该实例会以 `null` 值完成。此功能对于诸如读取后关闭文件之类的任务也很有用。

## 使用 StreamView 最小化对 Stream 类成员的访问

`StreamView` 类是 Stream 类的包装器，仅公开原始类中的 `isBroadcast` getter、`asBroadcastStream` 和 `listen` 方法。因此，如果你需要在代码中使用清晰的 `Stream` 接口，可以这样使用：

```
import 'dart:async';

main() {
  // some sample data
  var data = [1,2,3,4,5];
  // create the stream
  var stream = new Stream<num>.fromIterable(data);
  // Create a view
  var streamView = new StreamView(stream);
  // Now listen stream view like stream
  var subscriber = streamView.listen((data) {
    print(data);
  }, onError: (error){
    print(error);
  }, onDone: () {
    print('done');
  });
}
```

你将得到以下结果：

- 1
- 2
- 3
- 4
- 5
- done

## Sink 和 EventSink 接口

`Sink` 类表示数据接收器的通用接口。它定义了 `add` 方法，该方法会将数据放入 sink，以及 `close` 方法，该方法告诉 sink 未来不会再添加数据。`EventSink` 类使用 `Sink` 类的 `add` 方法向流发送数据事件，以及 `close` 方法发送完成事件。`addError` 方法属于 `EventSink` 类，用于向流发送异步错误。

## StreamConsumer 接口的重要性

我们可以通过 `Stream` 类的 `pipe` 方法将一个流绑定到另一个流。消费者流由 `StreamConsumer` 接口表示。此接口定义了两个流之间的契约。`addStream` 方法用于消费源流的元素。消费者流将监听源流并对每个事件执行某些操作。它可能在错误后停止监听，也可能消费所有错误并在完成事件时停止。`close` 方法告诉消费者未来不会再添加流。此方法返回 `Future` 实例，当源流已被消费且消费者已关闭时完成。

## StreamSink 类的作用是什么？

`StreamSink` 类结合了 `StreamConsumer` 和 `EventSink` 的方法。你应该知道这两个类的 方法会相互阻塞。我们不能在通过 `StreamConsumer` 的 `addStream` 方法添加源流的同时，通过 `EventSink` 类的方法发送数据或错误事件。我们只能在 `addStream` 方法返回的 `Future` 实例完成后再开始使用 `EventSink` 的方法。此外，`addStream` 方法将被延迟，直到底层系统消费 `EventSink` 方法添加的数据。`StreamSink` 类有一个 `done` getter，它返回一个 `Future` 实例，当拥有的 `StreamSink` 类在以下任一条件下结束时完成：

- 它因在 `EventSink` 类的 `add`、`addError` 或 `close` 方法中添加事件而以错误完成
- 当所有事件已被处理且 sink 已关闭，或 sink 已停止处理更多事件时，它以成功完成

## 使用 StreamTransformer 类转换流

`StreamTransformer` 类帮助你创建一个新的消费者流，该流通过 bind 方法绑定到原始流。`StreamTransformer` 类可以通过两个工厂构造函数实例化，这两个构造函数定义了转换发生的不同策略。在以下工厂构造函数中，我们需要指定以下特殊的转换器函数：

```
const factory StreamTransformer (Function StreamSubscription<T>
  transformer(Stream<S> stream, bool cancelOnError))
```

转换器函数接收一个绑定流作为参数并返回 `StreamSubscription` 类的实例。如果你计划实现自己的流转换器函数，它看起来像这样：

```
import 'dart:async';

void main() {
  // some sample data
  var data = [1,2,3,4,5];
  // create the stream
  var stream = new Stream<num>.fromIterable(data);
  // Create StreamTransformer with transformer closure
  var streamTransformer =
    new StreamTransformer<num, num>(doublingTransformer);
  // Bound streams
  var boundStream = stream.transform(streamTransformer);
  // Because we start listening the 'bound' stream the
  // 'listener' method invokes the 'doublingTransformer'
  // closure
  boundStream.listen((data) {
    print('$data');
  });
}

StreamSubscription doublingTransformer(Stream<num> input,
  bool cancelOnError) {

  StreamController<num> controller;
  StreamSubscription<num> subscription;
  controller = new StreamController<num>(
    onListen: () {
```

```
```dart
subscription = input.listen((data) {
  // Scale the data double.
  controller.add(data * 2);
},
onError: controller.addError,
onDone: controller.close,
cancelOnError: cancelOnError);
});
return controller.stream.listen(null);
}
```
```

前面的代码生成了以下输出：

```
```
2
4
6
8
10
```
```

另一个工厂方法创建了一个 `StreamTransformer` 类，该类将事件委托给特殊函数，这些函数处理数据、错误和完成事件，如以下代码所示：

```
```dart
factory StreamTransformer.fromHandlers({
    void handleData(S data, EventSink<T> sink),
    void handleError(Object error, StackTrace stackTrace,
        EventSink<T> sink),
    void handleDone(EventSink<T> sink)})
```
```

前一个示例的修改版本如下：

```
```dart
import 'dart:async';

void main() {
  // some sample data
  var data = [1,2,3,4,5];
  // create the stream
  var stream = new Stream<num>.fromIterable(data);
  // Create StreamTransformer with transformer closure
  var streamTransformer = new StreamTransformer<num, num>
      .fromHandlers(
        handleData:handleData,
        handleError:handleError,
        handleDone:handleDone);
  // Bound streams
```




当 `moveNext` 方法返回的 `Future` 以 `true` 完成时，且仅在下一次迭代之前，该值有效。`StreamIterator` 类是一个抽象类，只能通过工厂构造函数实例化，如下所示：

```
factory StreamIterator(Stream<T> stream)
```

让我们修改上一个主题中的示例，使用 `StreamIterator`。我们将像之前一样从 `Iterable` 创建一个简单的流。然后，创建一个 `StreamIterator` 的实例。最后，使用 `forEach` 函数遍历该流，并调用闭包函数打印流的缩放元素，如下面的代码所示：

```
main() {
    // some sample data
    var data = [1,2,3,4,5];
    // create the stream
    var stream = new Stream<num>.fromIterable(data);
    // Create an iterator
    var iterator = new StreamIterator(stream);
    // Iterate over all elements of iterator and print values
    forEach(iterator, (value) {
        // Scale the data double.
        print(value * 2);
    });
}
```

实际上，这段代码看起来与我们之前使用迭代器的代码类似。所有的魔法都发生在 `forEach` 方法内部，如下面的代码所示：

```
forEach(StreamIterator iterator, f(element)) {
    return Future.doWhile(() {
        Future future = iterator.moveNext();
        future.then((bool hasNext) {
            if (hasNext) {
                f(iterator.current);
            }
        });
        return future;
    });
}
```

由于 `moveNext` 方法返回 `Future` 值，我们需要使用 `Future` 类的 `doWhile` 方法来执行迭代。`Future` 的布尔结果返回一个 `hasNext` 参数。我们一直调用闭包函数，直到 `hasNext` 参数的值为 `true`。

## 流框架

上述代码生成以下结果：

```
2
4
6
8
10
```

## 总结

现在你已经对流框架有了更好的理解，它是 Dart VM 的关键概念之一。

流自 UNIX 早期就已存在。它们已被证明是一种用小组件组合大型系统的可靠方式。Dart 流框架支持单订阅流和广播流，以便根据所需解决方案采用不同的方法。

在下一章中，我们将介绍集合框架，以及何时以及如何正确使用不同类型的集合框架。你还将学习如何根据使用模式、并发性和性能考虑来选择正确的数据结构。
