Now, to implement this new observable that emits only even dates, all we need to do is chain the `filter()` function to our previous observable in order to create a new observable that we can subscribe to finally print what we want. You can see the use of this `filter()` operator in the following diagram:

![](bbox=[0.05, 0.26, 0.9, 0.52])

As you can see in this diagram, on the created observable, we don't emit the dates where the seconds are odd numbers. For this reason, we don't have their circles. Now, let's change our code using the `map()` function to use the `filter()` operator as well. This can be easily done with the following code:

```
Bacon
    .interval(1000)
    .map(
        (i)=> new Date()
    )
    .filter(
        (date)=>date.getSeconds() % 2 == 0
    )
    .onValue((date)=>console.log(date));
```