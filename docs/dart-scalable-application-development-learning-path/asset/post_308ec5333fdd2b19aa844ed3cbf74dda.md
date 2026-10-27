
> The `whenComplete` method is the asynchronous equivalent of a `finally` block.

Now when we are finished with definitions, let's discuss the different factory constructors of `Future`.

## Future and Timer

Let's create a `Future` class containing the result of the calling computation asynchronously with the `run` method of the `Timer` class, as follows:

```
Future calc = new Future(computation);
calc.then((res) => print(res));
```

This Future class does not complete immediately. The Timer class adds the event to the event queue and executes the computation callback when the event is being processed in the event loop. If the result of the computation function throws an error, the returned Future is completed with an error. If the computation function creates another Future, the current one will wait until the new Future is completed and will then be completed with the same result.

## Future and Microtask

In the following code, the `Future` class is a scheduled task in the microtasks queue, which does not get completed immediately:

```
Future calc = new Future.microtask(computation);
calc.then((res) => print(res));
```

If the result of computation throws, the returned Future is completed with the error. If computation creates another Future, the current one will wait until the new Future is completed and will then be completed with the same result.

## Sync the Future class

It may sound paradoxical, but we can create a sync version of the `Future` class, as follows:

```
Future calc = new Future.sync(computation);
calc.then((res) => print(res));
```

The reason for this is that the Future immediately calls the computation function. The result of the computation will be returned in the next event-loop iteration.

## Future with a value

The Future class can be created with a specified value, as follows:

```
Future valueFuture = new Future.value(true);
valueFuture.then((res) => print(res));
```

Here, a Future returns the specified value in the next event-loop iteration.

## Future with an error

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

## Delaying the Future class

Sometimes, it may be necessary to complete Future after a delay. It can be done as follows:

```
Future calc = new Future.delayed(
    new Duration(seconds: 1), computation);
calc.then((res) => print(res));
```

The Future will be completed after the given duration has passed with the result of the computation function. It always creates an event in the event queue, and the event gets completed no sooner than the next event-loop iteration if the duration is zero or less than zero.

> The Future class must not change the completed value or the error to avoid side effects from listeners.

If Future doesn't have a successor, any error could be silently dropped. To prevent these cases, Dart usually forwards the error to the global error handler.

So, how can this problem be solved? We wrap our code within a try/catch block to catch all the uncaught exceptions and it works perfectly, as shown in the following code:

```
main() {
    try {
        runServer();
    } on Error catch(e) {
        // ...
    }
}
```

This solution is universal and can be used in similar situations, so we will generalize it via the creation of a separate wrapper function:

```
wrapper(Function body, {Function onError}) {
    try {
        body();
    } on Error catch(e) {
        if (onError != null) {
            onError(e);
        }
    }
}

main() {
    wrapper(runServer, onError: (e) {
        // ...
    });
}
```

The body argument represents any protected code and is covered within a try/catch block inside wrapper. A wrapper function uses the onError function to handle all the uncaught exceptions. Using a wrapper function is a good practice and its use is advised in other such situations. This is the zone.

> A zone is a configurable execution context that handles uncaught exceptions and asynchronous tasks.

Let's take a look at what zones can do:

- In critical situations, it allows you to handle exceptions properly
- It provides a way to handle multiple async operations in a single group
- It can have an unlimited number of nested zones, which behave like the parent one

Each zone creates a context, some kind of protected area, where the executing code exists. In addition to intercepting uncaught exceptions, zones can have local variables and can schedule microtasks, create one-off or repeating timers, print information, and save a stack trace for debugging purposes.

## Simple zone example

Let's transform our code to use a zone instead of the wrapper function, as follows:

```
import 'dart:io';
import 'dart:async';

main() {
    runZoned(runServer, onError: (e) {
        // ...
    });
}
```

The runZoned function is a code wrapper. By default, the async library implicitly creates a root zone and assigns it to a static current variable in the Zone class. So, we have an active zone that is always available to us inside the runZoned function. When the runZoned function runs, it forks the new nested zone from root one and executes the runServer function inside its context. Use the fork method of the current zone to create a new child of this one.

> A zone can be created only through the fork method of the current zone.

## Zone nesting

Let's say we have to serve static files in our server. So, we will need to read the file and serve it. To do this properly, we fork the nested zone and protect our code with the runZoned function, as follows:

```
runServer() {
    HttpServer
    .bind(InternetAddress.ANY_IP_V4, 8080)
    .then((server) {
        server.listen((HttpRequest request) {
            runZoned(() {
                readFile(request.uri.path).then((String content) {
                    request.response.write(content);
                    request.response.close();
                });
            }, onError: (e) {
                request.response.statusCode = HttpStatus.NOT_FOUND;
                request.response.write(e.toString());
                request.response.close();
            });
        });
    });
}

Future<String> readFile(String fileName) {
  switch (fileName.trim()) {
    case "/":
    case "/index.html":
    case "/favicon.ico":
      return new Future.sync(() => "Hello, world!");
  }
  return new Future.sync(() =>
    throw new Exception('Resource is not available'));
}
```

Inside the nested zone, we call the readFile function with a resource name and it returns the content. If the resource is not available, readFile generates an exception and the program catches it in the onError function, which is registered as the zone's error handler. If we don't specify the error handler, the exception will be bubbled up through the zone-nested hierarchy until any parent zone catches it or it reaches a top-level executable and terminates the program.

## Zone values

Now, it's time to discuss authentication on our server as some resources may not be available to the general public. We will follow the idea of token-based authentication that relies on a signed token that is sent to the server on each request. We will create a map of tokens to remember all the authorized clients, and then fork a new zone for authentication. We will then read the client token from the header that is to be used for authentication. When we get a map of tokens from the current zone, we will inject them into the zone via zoneValues, as shown in the following code:

```
runServer() {
  HttpServer
  .bind(InternetAddress.ANY_IP_V4, 8080)
  .then((server) {
    Set tokens = new Set.from(['1234567890']);
    server.listen((HttpRequest request) {
        runZoned(() {
            authenticate(request.headers.value('auth-token'));
        }, zoneValues: {'tokens': tokens}, onError: (e) {
            request.response.statusCode = HttpStatus.UNAUTHORIZED;
            request.response.write(e.toString());
            request.response.close();
        });
        runZoned(() {
            readFile(request.uri.path).then((String content){
                request.response.write(content);
                request.response.close();
            });
        }, onError: (e) {
            request.response.statusCode = HttpStatus.NOT_FOUND;
            request.response.write(e.toString());
            request.response.close();
        });
    });
  });
}
```

The authentication based on the existence of a token within tokens is as follows:

```
authenticate(String token) {
    Set tokens = Zone.current['tokens'];
    if (!tokens.contains(token)) {
        throw new Exception('Access denied');
    }
}
```

In the preceding code, we used the zone-local variables to track tokens and authenticate clients. Here, the variables were injected into the zone with the zoneValues argument of the runZoned function. Our tokens variable works like a static variable in the asynchronous context.

> The zone-local variables can play the role of static variables that are visible only in the scope of the zone.
