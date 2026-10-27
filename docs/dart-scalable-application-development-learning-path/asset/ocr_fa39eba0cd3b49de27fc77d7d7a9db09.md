# Transforming streams with the StreamTransformer class

The `StreamTransformer` class helps you create a new consumer stream that is bound to the original one via the bind method. The `StreamTransformer` class can be instantiated through two factory constructors that define different strategies on how the transformation will happen. In following factory constructor, we need to specify the following special transformer function:

```
const factory StreamTransformer (Function StreamSubscription<T>
  transformer(Stream<S> stream, bool cancelOnError))
```

The transformer function receives a bounded stream as an argument and returns an instance of the `StreamSubscription` class. If you are planning to implement your own stream transformer function, it will look like this:

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