
In the following example, we will use the skip() operator to bypass the first element in the Observable:

```
Rx.Observable
    .of(1,2,3)
    .skip(1)
    .subscribe((i)=>console.log(i));
```

If you run this code, it will print the following:

2
3

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_144_0.png)

This operator can be used with the take() operator to extract a range from the original Observable:

```
Rx.Observable
    .of(1,2,3,4)
    .skip(1)
    .take(2)
    .subscribe((i)=>console.log(i));
```

In this example, we create an Observable from the values 1, 2, 3, and 4. We then skip the first, so now we have elements: 2 and 3. We finally take only two of them, so if we run this code, we will see the following output:

2
3

## The skipWhile() operator

The last operator in this chapter, the skipWhile() operator, is analogous to the takeWhile() operator, but it skips the elements while a given conditional function is satisfied. It has the following signature:

```
observable.skipWhile(condition, [context]);
```

The first parameter is mandatory and the second is optional:

- condition: This is a function to test the elements to be skipped
- context: This is an argument to be used in the condition function

To illustrate this operator, we can use the same example from the takeWhile(), but using the skipWhile() function instead:

```
Rx.Observable
    .of(2,4,5,6)
    .skipWhile((i)=>i%2===0)
    .subscribe((i)=>console.log(i));
```

In this code, we will skip the even elements starting from the beginning of the sequence and stop when we find the first non-even number. So if we run this code, we will see the following output:

5
6

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_146_0.png)

## Summary

In this chapter, we learned how to deal with massive amounts of data. We called this the problem of backpressure. We can use two different approaches to deal with backpressure: lossy strategies and loss-less strategies.

We also learned about the advantages and disadvantages of each approach, and also which operators we can use to mitigate them.

At the end of the chapter, we also received more tools to query for data in our observables, such as the `first()` operator, some `take()` operators, and some `skip()` operators.

In the next chapter, we will learn about operators to combine different sources of data.

## 6

## Too Many Sources - Combining Observables

In the last chapter, we learned how we can use different operators to deal with the backpressure problem, learned two different strategies to deal with this problem:

- Lossy strategy
- Loss-less strategy

For each strategy, we learned different operators implement a lossy strategy to deal with the back pressure problem. learned the following operators:

- `throttle()`
- `sample()`
- `debounce()`
- `pause()`

To implement a loss-less strategy to deal with the back pressure problem we learned the following operators:

- `bufferWithCount()`
- `bufferWithTime()`
- `bufferWithTimeOrCount()`
- `pause()` using buffering
- `controlled()`

We also learned when to use each strategy, based on the amount of memory we have available and if we can afford losing any data or not.

For the last, we learned new operators to filter data, along with the filter() operator. This operator lets us avoid computation of unnecessary events on an observable. We learned the following operators to filter data:

- `first()`
- `take()`
- `takeLast()`
- `takeWhile()`
- `skip()`
- `skipWhile()`

If you did not familiarize yourself with the problem of backpressure. Take time to read this chapter again, as this problem is very common and anyone calling themselves a master in functional reactive programming must be able to recognize and mitigate this problem.

In this chapter, we will see how we can combine different observables to create new observables, we will also use more complex and elaborate examples to teach the new concepts to get you out of your comfort zone and improve your functional reactive programming skills even more.

In this chapter, we will learn the following topics:

- When do I need to combine observables?
- Running observables in parallel
- Concatenating observables
- Combining data from different observables
- Reusing methods and operators from previous chapters in more complex examples

## When do I need to combine observables?

One of the main advantages of functional reactive programming is how easy it makes reusing code, as we detach the source of data (observable) to the transformations we do on this data (operators) to the effect it causes when it happens (action taken by an observer), we can easily reuse each part of our code.

In the previous chapters, we saw how we can reuse an observable adding multiple observers to it. Now we can go one step further and combine these observables to create a new source of data; this will give us even more power.

When we combine (specifically when we concatenate) observables we can also avoid repeating code that is aligned with one of the main concepts of good code, which is **Don't Repeat Yourself (DRY)**.

One simple and easy to understand example of combining observables to avoid subscribing to two observables can happen in a common task. Imagine that we want to implement a search tool in a website. On our search tool we want to do the search when the user hits *Enter* (and the search box has focus) or when the user clicks on the search button. Using RxJS it's easy to create an observable for clicks in the enter button for the search box using the `fromEvent()` method, and it's also easy to create an observable from clicks in the search button with the same method. Without the ability to concatenate both observables we would have to subscribe to each observable creating two different subscriptions. This is not only unnecessary, but it can also lead to errors as any subscription can be individually terminated. But as we will see in this chapter, we can concatenate both observables to create a new observable that emits data when any of the sources emits, and we will subscribe only to this observable to create code that is more readable, error prone, and semantically correct.

We also learned that we can use observables, to help us with the flow control of our application, using it to wrap promises or even callbacks. So we can also combine observables to run these asynchronous computations in parallel.

## Running observables

Dealing with asynchronous computation in JavaScript is a hard task, and this is because all your code runs in a single thread. So, to keep this thread available most operators dealing with I/O use a callback to return the control to your program when the data is available.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_149_0.png)

> The concept of threads and how JavaScript virtual machines work internally goes beyond the scope of this book, and it is not necessary to understand it.

The extensive use of callback functions usually leads to a problem called **callback hell**, which is basically a code with too many callbacks making it really hard to read from a programmer perspective. To mitigate this problem, promises are now part of the standard JavaScript and are implemented by all modern browsers. As promises are composable by nature, it makes your code cleaner and gives you a safe way out from callback hell.

But promises have only basic constructs to compose with each other, and for this reason RxJS (and most functional reactive programming libraries in JavaScript) gives you built-in methods to turn a promise in an observable making it even more composable with the usage of operators.

With that said, it is easy to see why functional reactive programming, with the power of concatenating and combining observables (which can be asynchronous computation), is a natural solution to deal with your application flow control.

## Concatenating observables

RxJS enables some operators to combine several observables in a single one containing the events from all, the most common operators are `concat()` and `merge()`, which simply creates a new observable containing all data from the others, but in this chapter we will also learn how to do more interesting combinations with other operators.

## Using the concat() operator

The most common operator to concatenate two sources of data (observables) is the `concat()` operator. This operator receives multiple observables as arguments and concatenates all observables from the left to right. It preserves the order of the elements in the observables and only propagates data from the next observable if the current one is already terminated.

This operator has the following signature:

```
observable.concat(observables);
```

It accepts any number of arguments, but they all must be observables:

- `observable`: It is any number of observables
