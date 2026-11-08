This is a class() method instead of an instance() method, and it receives an arbitrary number of arguments and the last is optional:

+   - observables: An arbitrary number of observable sequences to be executed

![](bbox=[0.07, 0.2, 0.14, 0.26])

It can also accept a last argument to concatenate the items of the produced array, but this is not a common use case as you can easily do it using the map() operator over the array.

The forkJoin() operator preserves the order of the observables and an example of this usage can be seen in the following code:

```
Rx.Observable
  .forkJoin(
    Rx.Observable.fromPromise(Promise.resolve(1)),
    Rx.Observable.just(2)
  ).subscribe((k)=>console.log(k));
```

If you run this code, you will see the following output:

[ 1, 2 ]

As we discussed previously, this operator propagates only the last value of each observable sequence, which makes it perfect for promises and callbacks (which emit only one value), but we can use it with any Observable.

We can change our code to use an infinite Observable and see what happens:

```
Rx.Observable
  .forkJoin(
    Rx.Observable.fromPromise(Promise.resolve(1)),
    Rx.Observable.interval(100)
  ).subscribe((k)=>console.log(k));
```

If you run this code, you will see nothing printed in the console and your program also never ends. This happens because the second parameter is an infinite observable, so the forkJoin() operator will wait for it to finish to propagate all values.