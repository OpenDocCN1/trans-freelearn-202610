The preceding code snippet generates the following result:

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

These results reaffirm the fact that the broadcast stream doesn't guarantee the order of the delivery events to different listeners. It only guarantees an order of the delivery events inside each listener.

# What does the StreamSubscription class do?

The listen method of the Stream class adds the following subscription to the stream:

```
StreamSubscription<T> listen(
  void onData(T event),
  { Function onError, void onDone(), bool cancelOnError});
```

A callback function onData is called every time when a new data event comes from this stream. Existence of this function is important because without it nothing will happen. The optional onError callback is called when an error comes from the stream. This function accepts one or two arguments. The first argument is always an error from the stream. The second argument, if it exists, is a StackTrace instance. It can be equal to null if the stream received an error without StackTrace itself. When the stream closes, it calls the onDone callback function. The cancelOnError flag informs the subscription to start the cancellation the moment the error occurs.