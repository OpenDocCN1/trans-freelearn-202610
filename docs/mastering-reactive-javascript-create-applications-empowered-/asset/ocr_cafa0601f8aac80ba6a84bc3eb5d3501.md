In this example, we just changed the concat() operator for the merge() operator, but as we have no infinite observable nor an asynchronous one, we can't see any difference on the output, as follows:

```
1
2
```

Now let's change the previous example, which uses an Observable created with the interval() method:

```
Rx.Observable
.just(1)
.merge(Rx.Observable.interval(1000))
.merge(Rx.Observable.just('Hello World!!!'))
.subscribe((k)=>console.log(k));
```

In the previous session we saw the output from a code like this, but using concat() instead of merge(), and in that example the Hello World!!! message doesn't get printed because the Observable created from interval() never finishes and as the concat() operator preserves the order of the observables, it never propagates data from the last. But the merge() operator preserves the order when the data is propagated and for this reason when we run this code we see this output in the console:

```
1
Hello World!!!
0
1
2
//Keeps printing sequential numbers until we stops the program
```

This code can be explained by the following diagram: