The tests with arrays instead of observables are a lot easier to understand given their synchronous nature. On the first, we create an empty array, apply the transducer to it, and check if the result is empty, while in the second we create an array containing the elements we want to test and do three assertions to ensure that it returns the expected result. If we run our test suite with our new test cases we will see the following output in our console:

```
bash-3.2$ mocha example9_7_test.js

Transducer test
  ✓ should return empty when running for empty observables
  ✓ should propagates 2,4 when running for an observable containing 1,2,3,4
  ✓ should return empty when running for empty data sources
  ✓ should propagates 2,4 when running for a data source containing 1,2,3,4

4 passing (15ms)
```

It shows that all tests have passed, as we expected.

With transducers we can easily test the transformations we want to apply in our data, and it is even easier as we can use synchronous data structures to test, such as arrays, instead of asynchronous ones, such as observables.

# A performance comparison for JavaScript code using transducers

One of the advantages of using transducers with RxJS is the performance improvement you can get from this, so far we have just commented on this improvement, now we will add some benchmarks so you can see the performance benefits for yourself. The results presented here are based on running on my personal computer; executions on different computers might lead to different results.

The most used and famous library for running benchmarks is called benchmarks. We can install it to run in Node.js environments using npm. To add it to your project just type the following command:

```
npm install benchmark@2.1.2
```