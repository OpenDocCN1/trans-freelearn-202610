All these operators create an observable containing the first four natural numbers that are not multiples of 3, for this reason, option 2 (1,2,4, and 5), is the correct answer.

# Aggregating data

The most important operator to aggregate data is the reduce() operator, as the other operators the reduce() operator returns a new observable; in this case the observable returned contains a single value that is the result of the execution of a function over all the elements of the original observable.

We can use the reduce() operator to find the lower numeric value in an observable:

```
```js
Rx.Observable
.of(1,5,0,9,56,-2,13)
.reduce(function(a,b){
  if(a<b){
    return a;
  }else{
    return b;
  }
})
.subscribe((i)=>console.log(i));
```
```

In this example, the values from the observable are going to be scanned from left to right, and the reduce() function is used to check which one is lower (the if/else block), and then we log the result. Thus, as expected the result is:

-2

![](bbox=[0.08, 0.7, 0.16, 0.78])

The reduce() operator can be used for mathematical operations; actually, RxJS contains some mathematical operators such as min, max, and sum and each one can be easily implemented using the reduce() operator. You can also use the reduce() operator for other operations such as creating an array, a new object, concatenating strings, and so on; every time you need to aggregate data the reduce() operator can be your ally.

# Going beyond the basics

RxJS supplies a lot more than just operators to do simple transformation in your data, we learned some more complex usages for it and we will review the most important of it.