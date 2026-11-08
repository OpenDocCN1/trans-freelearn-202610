# Reusing observables

In the previous section, we saw how we can use operators to create new observables, but until this moment, we have never tried reusing an observable. To do this, I propose a change in the program we created in the previous section. We still want to print the string for every date where the seconds part is an even number. But now, let's also print the string a second has passed every second. To do this, we can use another Bacon.interval call to generate a new observable that emits an empty object every second. Once this is done, we can map each object in the observable to the string we want and finally subscribe to print, as you can see in the following code:

```
Bacon
    .interval(1000)
    .map(
        ()=> 'a second has passed'
    )
    .onValue((str)=> console.log(str));
```

If you add the preceding code to the last program, you will see the following output:

```
a second has passed
The number in the second part of the date 2016-11-23T22:42:18.683Z
    is 18 which is as even number
a second has passed
a second has passed
The number in the second part of the date 2016-11-23T22:42:20.696Z
    is 20 which is as even number
a second has passed
a second has passed
The number in the second part of the date 2016-11-23T22:42:22.702Z
    is 22 which is as even number
```

This solution is okay, but we can reuse the first Bacon.interval(), storing it in a variable. This way, we don't have to keep repeating this code every time we want a source of events emitting every second, using a variable as follows:

```
var emitsEverySecondStream = Bacon.interval(1000);
```

We can create better code by reusing this variable as follows:

```
var emitsEverySecondStream = Bacon.interval(1000);
emitsEverySecondStream.map(
    (i)=> new Date()
)
.filter(
    (date)=>date.getSeconds() % 2 == 0
)
```