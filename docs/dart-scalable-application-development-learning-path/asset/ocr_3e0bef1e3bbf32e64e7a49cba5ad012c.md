# Future with a value

The Future class can be created with a specified value, as follows:

```
Future valueFuture = new Future.value(true);
valueFuture.then((res) => print(res));
```

Here, a Future returns specified value in the next event-loop iteration.

# Future with an error

The Future class can be created with an error, as follows:

```
try {
    throw new Error();
} on Error catch(ex, stackTrace) {
    Future errorFuture = new Future.error(ex, stackTrace);
    errorFuture.catchError((err, stack) => print(err));
}
```

This Future completes with an error in the next event-loop iteration.

# Delaying the Future class

Sometimes, it may be necessary to complete Future after a delay. It can be done as follows:

```
Future calc = new Future.delayed(
    new Duration(seconds:1), computation);
calc.then((res) => print(res));
```

The Future will be completed after the given duration has passed with the result of the computation function. It always creates an event in the event queue, and the event gets completed no sooner than the next event-loop iteration if the duration is zero or less than zero.

> The Future class must not change the completed value or the error to avoid side effects from listeners.

If Future doesn't have a successor, any error could be silently dropped. In preventing these cases, Dart usually forwards the error to the global error handler.