
```dart
print('happy end');
}
```

The output is as follows:

**happy end**

a

c

d

**Error occurred in b!**

- The following code helps to catch a specific error or more than one error:

  ```dart
  firstStep()
      .then((_) => secondStep())
      // more .then( steps )
      .catchError(handleArgumentError,
          test: (e) => e is ArgumentError)
      .catchError(handleFormatException,
          test: (e) => e is FormatException)
      .catchError(handleRangeError,
          test: (e) => e is RangeError)
      .catchError(handleException, test: (e) => e is Exception);
  ```

- Often, you want to execute a method that does a cleanup after asynchronous processing no matter whether this processing succeeds or ends in an error. In that case, use `whenComplete`:

  ```dart
  firstStep()
      .then((_) => secondStep())
      .catchError(handleError)
      .whenComplete(cleanup);
  ```

With respect to handling synchronous and asynchronous errors, let's suppose that we want to call a function `mixedFunction`, with a synchronous call to `synFunc` that could throw an exception and an asynchronous call to `asyncFunc` that could do likewise, as shown in the following code:

```dart
mixedFunction(data) {
  var var2 = new Var2();
  var var1 = synFunc(data);        // Could throw error.
  return var2.asyncFunc().then(processResult);  // Could throw error.
}
```

If we call this function `mixedFunction(data).catchError(handleError);`, then `catchError` cannot catch an error thrown by `synFunc`. To solve this, they call in a `Future.sync` function as shown in the following code:

```dart
mixedFunction(data) {
  return new Future.sync(() {
    var var1 = synFunc(data);        // Could throw error.
    return var1.asyncFunc().then(processResult);  // Could throw error.
  });
}
```

That way, `catchError` can catch both synchronous and asynchronous errors.

### How it works...

In variation 1, `catchError` will handle all errors that occur in any of the executed steps. For variation 2, we make a list with all the steps. The `Future.wait` option will do exactly as its name says: it will wait until all of the steps are completed. But they are executed in no particular order, so they can run concurrently. All of the functions are triggered without first waiting for any particular function to complete. When they are all done, their return values are collected in a list (here called `val`) and can be processed. Again, `catchError` handles any possible error that occurs in any of the steps.

In the case of an error, the List value is not returned; we see that, in the example on `wait_error`, happy end is first printed, then a, c, and d complete, and then the error from b is caught; if d also throws an error, only the b error is caught. The `catchError` function doesn't know in which step the error occurred unless that is explicitly conveyed in the error.

In the same way as in the catch block, we can also test in `catchError` when a specific exception occurs using its second optional `test` argument, where you test the type of the exception. This is shown in variation 3; be sure then, to test for a general exception as the last clause. This scenario will certainly be useful if a number of different exceptions can occur and we want a specific treatment for each of them.

Analogous to the optional `finally` clause in a try statement, asynchronous processing can have a `whenComplete` handler as in variation 4, which always executes whether there is an error or not. Use it to clean up and close files, databases, and network connections, and so on.

Finally, in variation 5, the normal `catchError` function won't work, because it can only handle exceptions arising from asynchronous code execution. Use `Future.sync` here, which is able to return the result or error from both synchronous and asynchronous method calls.

## Scheduling tasks using Futures

The Dart VM is single-threaded, so all of an app's code runs in one thread, also called the main isolate. This is because `main()` is the function where Dart code starts executing an isolate, because Dart's concurrency model is based on isolates as separate processes that exchange messages. We will talk about isolates in depth in the coming recipes, but if your code doesn't start a new isolate, all of it runs in one isolate. But, in this one isolate, you can have lots of asynchronous pieces of code (let's call them tasks) running at the same time; in what order do they execute, and can we influence that order? It turns out that a better understanding of Dart's event loop and task queuing mechanism enables us to do that. This recipe will clarify Dart's scheduling mechanism and give you hints and tips for an ordered execution of tasks.

### How to do it...

Have a look at the program `tasks_scheduling.dart` (the tasks are numbered consecutively and according to the way they are started):

```dart
import 'dart:async';

main() {
  print('1) main task #1');
  scheduleMicrotask(() => print('2) microtask #1'));
  new Future.delayed(new Duration(seconds: 1),
      () => print('3) future #1 (delayed)'));
  new Future(() => print('4) future #2'));
  print('5) main task #2');
  scheduleMicrotask(() => print('6) microtask #2'));
  new Future(() => print('7) future #3'))
    .then((_) => print('8) future #4'))
    .then((_) => print('9) future #5'))
    .whenComplete(cleanup);
  scheduleMicrotask(() => print('11) microtask #3'));
  print('12) main task #3');
}

cleanup() {
  print('10) whenComplete #6');
}
```

The following screenshot shows the output of the program, the order of which is explained in the next section:

```text
1) main task #1
5) main task #2
12) main task #3
2) microtask #1
6) microtask #2
11) microtask #3
4) future #2
7) future #3
8) future #4
9) future #5
10) whenComplete #6
3) future #1 (delayed)
```

### How it works...

The main isolate proceeds as follows:

1. First the code in `main()` is executed, line after line and synchronously.
2. Then the event-loop mechanism kicks in, and this looks at the two queues in the system in the following order:
   - First, it takes a look at the microtask queue. This queue is for all tasks that need to be executed before the event queue, for example, the changes that have to be done internally before the DOM starts rendering the modified screen. All of the tasks in this queue are executed before going to the following step.
   - Then, the event queue is handled. Here, the tasks of all outside events, such as mouse events, drawing events, I/O, timers, messages between Dart isolates, and so on, are scheduled. Of course, in each queue, the tasks are handled one by one in the first in first out order.

> While the event-loop is handling the microtask queue, no work is done on the event-queue, so the app can't draw or react to user events and seems effectively blocked. So, keep the microtask queue as short as possible. Preferably, put your tasks on the event queue.

In principle, when both queues are empty, the app can exit. Tasks (these are pieces of code to run later, asynchronously) can be scheduled using the following classes and methods from `dart:async`:

1. Make a new `Future` object with a function to execute; this is appended to the event queue.
2. With `Future.delayed`, you can specify the execution of a function to occur after a certain duration; this also goes to the event queue.
3. Call the top-level `scheduleMicrotask()` function, which appends an item to the microtask queue.

> Don't use a `Timer` class to schedule a task; this class has no facilities to catch exceptions, so an error during a timer task will blow up your app.

Chaining Futures in a series of `then` statements effectively ensures that they are executed in that order (see step 1 of the previous recipe). Also, a `whenComplete` clause will execute immediately after all the previous `then` statements.

So this is the order of execution: `main()`, then the microtask queue, then the event queue, and then delayed tasks.

> As a general best practice, don't put a compute-intensive task on either queue, but create that task in a separate isolate (or worker for a web app).

### See also

- See the upcoming recipes about isolates in this chapter, such as *Using isolates in web apps* and *Using multiple cores with isolates*

## Running a recurring function

Let's suppose your code needs to run a certain function periodically at a certain interval. This recipe shows how you can do this very easily.

### How to do it...

Look at the following code of `recurring_function.dart`:

```dart
import 'dart:async';
var count = 0;
```
