This operation can be represented by the following diagram:

![](bbox=[0.19, 0.15, 0.85, 0.47])

We can also supply an initial value for the reduce() operator, as can be seen in the following example:

```
Rx.Observable
.of("w","o","r","l","d")
.reduce((acc,current)=>acc+current, "Hello ")
.subscribe((i)=>console.log(i));
```

As you can imagine, running this code will concatenate the whole string so you could see the following message on your console:

```
Hello world
```

# Summary

In this chapter, we learned how to apply operators to observables. This helps decouple our code and improve its testability and readability. This is the first time we used the following operators in RxJS such as map(), flatMap(), filter(), and reduce().