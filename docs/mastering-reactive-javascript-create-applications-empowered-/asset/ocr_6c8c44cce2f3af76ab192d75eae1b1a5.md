The use of these operators in this example is simple. We only call each operator for the three observables and `subscribe()` to listen to data (or an error) in each of them so that we can compare the outputs. When you execute this program, you will see the following output in your console:

```
onErrorResumeNext operator propagated ===> 1
onErrorResumeNext operator propagated ===> Hello
onErrorResumeNext operator propagated ===> Hello again
catch operator propagated===> 1
catch operator propagated===> Hello
```

Looking at this output it's easy to see that the `catch()` operator has the same behavior as the `try/catch` block from synchronous computation, while the `onErrorResumeNext()` attempts to execute all observables.

![](bbox=[0.08, 0.34, 0.15, 0.4])

The `catch()` operator tries an observable and handles the execution to the next only when it fails, while the `onErrorResumeNext()` executes all observables from left to right, passing to the next when the current one completes or fails.

# The retry() operator

This operator again lets you execute an `observable` a given number of times in the presence of an error. The most common use case is to do an HTTP call to a resource and retry a couple of times if an error happens. The `retry()` operator has the following signature:

```
observable.retry(retryAttempts);
```

It receives only one parameter:

+   - `retryAttempts`: This is a number indicating the number of times this `observable` must be retried.

To test this operator we will try to fetch the Google search page and retry it three times before propagating the error (you can change the Google page for any other URL). To do so, we will use the `request-promise` library, which fetches a URL using `promise()`, and then use the `fromPromise()` method to create an `Observable` from that `promise()`. First, we need to install the `request-promise` and the `request` library for node:

```
npm i request@2.79.0 --save
npm i request-promise@4.1.1 --save
```