> The `whenComplete` method is the asynchronous equivalent of a `finally` block.

Now when we are finished with definitions, let's discuss the different factory constructors of `Future`.

# Future and Timer

Let's create a `Future` class containing the result of the calling computation asynchronously with the `run` method of the `Timer` class, as follows:

```
Future calc = new Future(computation);
calc.then((res) => print(res));
```

This Future class does not complete immediately. The Timer class adds the event to the event queue and executes the computation callback when the event is being processed in the event loop. If the result of the computation function throws an error, the returned Future is completed with an error. If the computation function creates another Future, the current one will wait until the new Future is completed and will then be completed with the same result.

# Future and Microtask

In the following code, the `Future` class is a scheduled task in the microtasks queue, which does not get completed immediately:

```
Future calc = new Future.microtask(computation);
calc.then((res) => print(res));
```

If the result of computation throws, the returned Future is completed with the error. If computation creates another Future, the current one will wait until the new Future is completed and will then be completed with the same result.

# Sync the Future class

It may sound paradoxical, but we can create a sync version of the `Future` class, as follows:

```
Future calc = new Future.sync(computation);
calc.then((res) => print(res));
```

The reason for this is that the Future immediately calls the computation function. The result of the computation will be returned in the next event-loop iteration.