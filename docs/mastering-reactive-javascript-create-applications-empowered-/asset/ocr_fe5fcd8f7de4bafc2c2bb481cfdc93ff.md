We also learned when to use each strategy, based on the amount of memory we have available and if we can afford losing any data or not.

For the last, we learned new operators to filter data, along with the filter() operator. This operator lets us avoid computation of unnecessary events on an observable. We learned the following operators to filter data:

+   - `first()`
+   - `take()`
+   - `takeLast()`
+   - `takeWhile()`
+   - `skip()`
+   - `skipWhile()`

If you did not familiarize yourself with the problem of backpressure. Take time to read this chapter again, as this problem is very common and anyone calling themselves a master in functional reactive programming must be able to recognize and mitigate this problem.

In this chapter, we will see how we can combine different observables to create new observables, we will also use more complex and elaborate examples to teach the new concepts to get you out of your comfort zone and improve your functional reactive programming skills even more.

In this chapter, we will learn the following topics:

+   - When do I need to combine observables?
+   - Running observables in parallel
+   - Concatenating observables
+   - Combining data from different observables
+   - Reusing methods and operators from previous chapters in more complex examples

# When do I need to combine observables?

One of the main advantages of functional reactive programming is how easy it makes reusing code, as we detach the source of data (observable) to the transformations we do on this data (operators) to the effect it causes when it happens (action taken by an observer), we can easily reuse each part of our code.