
Before we start to use the library to benchmark transducers, let's see how we can use it through an example. Let's compare the difference between calling a function to sum two numbers using call or apply:

```
var Benchmark = require('benchmark')
var suite = new Benchmark.Suite;

var sum = (a,b)=>a+b;

suite.add('Sum using call', function() {
  sum.call(this,1,2);
})
.add('Sum using apply', function() {
  sum.apply(this,[1,2]);
})
.on('cycle', function(event) {
  console.log(event.target.toString());
})
.on('complete', function() {
  console.log('Fastest is ' + this.filter('fastest').map('name'));
})
.run();
```

In our example, we do the following steps:

- 1. Require the benchmark module.
- 2. Create a new `suite` of tests and store it in the `suite` variable.
- 3. Create the function we want to benchmark.
- 4. We must add functions to be tested in our `suite`; to do this we call the `add()` method, passing a string to identify this test, and the function to be tested. In the first test we want to see the performance of calling the `sum()` function using `call`.
- 5. The next test added to the `suite` runs the same function using `apply`.
- 6. We add a listener to the `cycle` event, which is called every time one cycle of benchmarks is finalized; we add a console log to show the performance results of this test.
- 7. We add a listener to the `complete` event, which is called when our test `suite` finishes; here we log the `fastest` test.
- 8. We run our performance test.

I named this file benchmark_example_1.js, so to run this performance test you must execute the following command:

```
node benchmark_example_1.js
```

When executing this program, it will take a few seconds before you see anything printed in the console; don't worry, benchmark.js is running the first test of our suite, and as soon as it is available it will print the results as follows:

```bash
bash-3.2$ node benchmark_example_1.js
Sum using call x 56,758,714 ops/sec ±0.37% (94 runs sampled)
Sum using apply x 37,697,012 ops/sec ±0.84% (87 runs sampled)
Fastest is Sum using call
```

The first two lines show the execution of each test of our suite; the first number shows the number of operations it could do per second (higher is better), the second, the percentage, is the margin of error of this result, and the last is the number of samples it needed to execute to find this margin of error. The last line prints the fastest method, and it shows the fastest method is to use call instead of apply.

In our execution it was able to execute the sum using call 56,758,714 times per second with a margin of error of 0.37%, and the test with apply 37,697,012 times per second with a margin of error of 0.84%; as we can see, the ops/sec of Sum using call is higher and for this reason it is faster.

Now let's see the difference of when transducers instead of regular transformations on RxJS. We will benchmark the same transducer containing a simple map() and filter() function. So we first must create the functions used to map and filter along with the values we want to propagate in our observable:

```javascript
var values = [1,2,3,4];
var mapFunction = (i)=>i+1;
var filterFunction = (i)=>i%2===0;
```

After this we must create the tests of our suite; the first test will be implemented using transducers:

```javascript
suite.add('Transducer', function(deferred) {
  var transducer = t.comp(
    t.map(mapFunction),
    t.filter(filterFunction)
  );
  Rx.Observable.fromArray(values)
    .transduce(transducer)
    .subscribe(function(){},function(){},function(){
deferred.resolve();
});
},{defer:true})
```

We first create our transducer, then an observable using the array we created. And as this is an asynchronous test we call deferred.resolve() when our test is concluded and we add the flag defer:true to mark our test as an asynchronous test.

Now we need to create the test using regular map() and filter() operators; it can be done as follows:

```
.add('Map/Filter', function(deferred) {
Rx.Observable.fromArray(values)
.map(mapFunction)
.filter(filterFunction)
.subscribe(function(){},function(){},function(){
deferred.resolve();
});
},{defer:true})
```

After this we must add the listeners to log the results of our tests as we did in our first example:

```
on('cycle', function(event) {
console.log(event.target.toString());
})
.on('complete', function() {
console.log('Fastest is ' + this.filter('fastest').map('name'));
})
.run();
```

Then we can execute the code, and see the results:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_235_0.png)

Looking at the results we can see the transducer is a little faster than the map()/filter() option, but let's see what happens if we change our array containing the values of the observables, so our observables propagate more data:

```
var values =
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25];
```

Now if we run our suite of tests we will see:

```
bash-3.2$ node benchmark_example_2.js
Transducer x 50,562 ops/sec ±0.69% (41 runs sampled)
Map/Filter x 37,451 ops/sec ±0.44% (43 runs sampled)
Fastest is Transducer
```

The number of operations per second is decreased in both, as is expected since we are processing more data. Perhaps the performance improvement from using transducers is a lot better, on our first execution the map()/filter() implementation was almost 7% slower than the transducer implementation, but in this execution it was almost 26% slower, so it's clear that you can have great performance improvements using transducers on big observables.

Looking at the results we can see real performance gains with the use of transducers.

## Summary

In this chapter, we learned what transducers are. They are a way to compose transformations to be applied to iterable sources, and they are independent of the data source.

We learned the advantages of using transducers–algorithmic transformation composition, performance improvement, better testability, better readability, and independency from the data source

In the advantages we learned how we can compose multiple transformations to create a transformation decoupled from the data source, and how we can reuse the same transducer in completely different data sources.

One section showed the advantage of testing a transducer, as it lets us use synchronous code to test, and how we can have a big performance improvement using this tool.

Now you are ready to create and implement your own applications using functional reactive programming; remember you had the advantage of using Reactive Extensions throughout this book, so most of the knowledge you learned here can be used in other platforms or languages as there are several implementations of Reactive Extensions on other platforms.

Functional reactive programming is an amazing way to model your application and to handle the flow of your data, and you have already mastered it. Congratulations.

Before we finish this book I felt that it was important to have a first hands-on with you on functional reactive programming, to do so I decided to create a web chat project using it. Web chat is a perfect candidate for functional reactive programming as it implies a continuous source of data flowing to and from your application.

In the next chapter, we will implement the server side of our web chat application, using nodes; we will go through the implementation and test of our code, so we will see how we can model our messages and listen to incoming messages from the client, and also how we can better deliver them.

I expect to use most of the concepts that we have learned in this book in the next two chapters, so let's create our first project.

## 10

## A Real-Time Server

In the last chapter, we covered an advanced topic in functional reactive programming, called the transducer. Transducers enable us to create a composition of transformations of data. One of the main advantages of transducers is the possibility of making your data transformations independent from the source of your data; with the `transducers-js` library we can use transducers on observables and in any iterable object.

With transducers, we can:

- Write better tests as it make it easy to decouple the source of the data to the transformations applied to it
- Improve the performance of our code; as it skips the need for any intermediate iterable
- Improve the maintainability of your code, when we compose transformations to create new transformations we can reuse them throughout our codebase, and avoiding code repetition is a good idea

Throughout this book, we have learned:

- What are observables?
- What is an operator?
- How can we listen to incoming data?
- How can we transform incoming data?
- How can we compose different sources of data?
- How can we reuse sources of data?
- How can we mitigate the problem of receiving data faster than we can process?
- Two different frameworks to implement functional reactive programming in JavaScript
