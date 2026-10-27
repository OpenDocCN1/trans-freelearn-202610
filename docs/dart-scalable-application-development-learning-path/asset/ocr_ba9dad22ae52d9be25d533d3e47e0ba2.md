# The Stream Framework

A result of this method is the instance of the `StreamSubscription` class. It provides events to the listener and holds the callback functions used in the listen method of the `Stream` class. You can set or override all the three callback functions via the `onData`, `onError`, and `onDone` methods of the `StreamSubscriber` class. The listening stream can be paused and resumed with the pause and resume methods. A special flag `isPaused` returns true if an instance of the `StreamSubscription` class is paused. The stream subscription can end with the cancel method at any time. It returns a `Future` instance, which completes with a `null` value when the stream is done cleaning up. This feature is also useful for tasks such as closing a file after reading it.

# Minimizing access to the Stream class
members using StreamView

The `StreamView` class is wrapper for the Stream class exposes only the `isBroadcast` getter, the `asBroadcastStream` and `listen` methods from original one. So if you need clear `Stream` interface in your code, you can use it like this:

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

You will get the following result:

+   1
+   2
+   3
+   4
+   5
+   done