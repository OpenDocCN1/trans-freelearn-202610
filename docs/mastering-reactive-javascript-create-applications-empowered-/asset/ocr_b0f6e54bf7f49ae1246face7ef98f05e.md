Now if were run our suite of tests we will see:

```
[bash-3.2$ node benchmark_example_2.js
Transducer x 50,562 ops/sec ±0.69% (41 runs sampled)
Map/Filter x 37,451 ops/sec ±0.44% (43 runs sampled)
Fastest is_Transducer
```

The number of operations per second is decreased in both, as is expected since we are processing more data. Perhaps the performance improvement from using transducers is a lot better, on our first execution the map()/filter() implementation was almost 7% slower than the transducer implementation, but in this execution it was almost 26% slower, so it's clear that you can have great performance improvements using transducers on big observables.

Looking at the results we can see real performance gains with the use of transducers.

# Summary

In this chapter, we learned what transducers are. They are a way to compose transformations to be applied to iterable sources, and they are independent of the data source.

We learned the advantages of using transducers–algorithmic transformation composition, performance improvement, better testability, better readability, and independency from the data source

In the advantages we learned how we can compose multiple transformations to create a transformation decoupled from the data source, and how we can reuse the same transducer in completely different data sources.

One section showed the advantage of testing a transducer, as it lets us use synchronous code to test, and how we can have a big performance improvement using this tool.

Now you are ready to create and implement your own applications using functional reactive programming; remember you had the advantage of using Reactive Extensions throughout this book, so most of the knowledge you learned here can be used in other platforms or languages as there are several implementations of Reactive Extensions on other platforms.

Functional reactive programming is an amazing way to model your application and to handle the flow of your data, and you have already mastered it. Congratulations.