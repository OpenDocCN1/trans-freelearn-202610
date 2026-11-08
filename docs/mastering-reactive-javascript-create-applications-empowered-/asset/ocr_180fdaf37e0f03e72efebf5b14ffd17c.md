If you run this code, you will see an output as follows:

```
18
37
//continues printing until you stop the program
```

![](bbox=[0.06, 0.19, 0.14, 0.25])

The numbers printed in the console may vary in different executions.

This operation is illustrated in the following diagram:

![](bbox=[0.12, 0.3, 0.78, 0.6])

We can also modify this code to use a sample() observable instead of a number:

```
Rx.Observable
  .interval(50)
  .sample(Rx.Observable.interval(1000))
  .subscribe((i)=>console.log(i));
```

Running this code will print the same output as the previous one.