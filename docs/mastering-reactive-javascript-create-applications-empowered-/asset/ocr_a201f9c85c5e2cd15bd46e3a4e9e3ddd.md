The first parameter is mandatory and the second is optional:

+   - `interval`: This is the time to wait in milliseconds
+   - `scheduler`: This is used to propagate the data

The first example for the `throttle()` operator uses an `Observable` created from an `interval()`. Now let's change that example to use the `debounce()` operator instead:

```
Rx.Observable
  .interval(50)
  .debounce(1000)
  .subscribe((i)=>console.log(i));
```

If you run this code, you will see nothing printed in the console because the `Observable` never stays 1000 milliseconds without emitting a value, but let's assume we can debounce 10 instead of 1000 as follows:

```
Rx.Observable
  .interval(50)
  .debounce(10)
  .subscribe((i)=>console.log(i));
```

In this case, you will see this printed in your console:

```
0
1
2
3
//continues printing until you stop the program.
```

This operation is illustrated in the following diagram:

![](bbox=[0.18, 0.61, 0.75, 0.84])