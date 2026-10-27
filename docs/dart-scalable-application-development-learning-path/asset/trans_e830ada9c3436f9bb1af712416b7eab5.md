

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

