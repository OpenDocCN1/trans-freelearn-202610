
An example usage of this method can be seen in following code:

```
var controllableObservable = Rx.Observable
  .interval(100)
  .controlled();

controllableObservable.subscribe((i)=>console.log(i));

controllableObservable.request(2);
setTimeout(()=>{
  console.log("Requesting more");
  controllableObservable.request(4);
},3000);
```

In this example, we create an Observable from an interval() and transform it to a controlled() observable with the controlled() method. Then we subscribe to it and request for two values (0 and 1 will be printed in the console). We then wait for 3000 milliseconds and ask for four more items (2, 3, 4, and 5).

If you run this code, you will see the following output:

```
0
1
Requesting more
2
3
4
5
```

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_135_0.png)

Now let's change our example to ask for more data than we have available in the Observable:

```
var controllableObservable = Rx.Observable
    .of(0,1)
    .controlled();
controllableObservable.subscribe((i)=>console.log(i));
controllableObservable.request(3);
```

In this example, our Observable contains only two items (0 and 1) and we will request for three items. If you run this code, you will see the following output:

0
1

So, if we ask for more data than we have available in our observable, it will just send all the data that it has available.

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_136_0.png)

The controlled() observable is a powerful tool as it gives the programmer full control of when and how many items he wants.

For the last example, let's see what happens if an error is propagated in a controlled() observable:

```
var subject = new Rx.Subject();

var controlledObservable = subject.controlled();

controlledObservable.subscribe(
  (i)=>console.log(i),
  (err)=>console.log('An error happened ' + err.message),
  ()=>console.log('Finished')
);

subject.onNext(0);
subject.onNext(1);
subject.onNext(2);

subject.onError(new Error(':('));
subject.onNext(3);
subject.onCompleted();

controlledObservable.request(5);
```

Using a subject, we can easily push data into the stream. This subject is the same used throughout this chapter to push errors. The only changes occur in the last line, where we request five items for our controlled() observable.

If you run this code, you will see the following output:

```
0
1
2
An error happened :(
```

Looking at the output, we see that the controlled() observable will pull the data (and run the observer) before the error happens and then propagates the error.

## More ways to filter data

In RxJS, we also have other operators to filter data. In this section, we will learn about the following operators:

- first()
- take()
- takeLast()
- takeWhile()
- skip()
- skipWhile()

We can use these operators when we are interested only in a subset of data from an observable.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_138_0.png)

There are other operators to filter data on RxJS but these are the most common ones.

## The first() operator

The first() operator returns a new observable containing only the first element in an observable which satisfies a given condition. This operator has the following signature:

```
observable.first([condition], [context], [defaultValue]);
```

All parameters are optional:

- `condition`: This is the function which the value must satisfy to be retrieved
- `context`: This is an argument used in the given conditional function
- `defaultValue`: This is a value used as the default if the observable is exhausted and no item satisfies the given condition

If no condition is supplied, this operator returns the first element in the Observable. This is shown in the following example:

```
Rx.Observable
  .of(1,2,3)
  .first()
  .subscribe((i)=>console.log(i));
```

If you run this code, you will see the following output:

1

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_139_0.png)

We can also supply a conditional function. In the following example, we will use the conditional function to retrieve the first even number on the Observable:

```
Rx.Observable
  .of(1,2,3)
  .first((i)=>i%2===0)
  .subscribe((i)=>console.log(i));
```

If you run this code, it will print the first even number in the observable, which is as follows:

2

We can also supply a default value if no element satisfies the condition:

```
var defaultValue = 'no even numbers';
Rx.Observable
  .of(1,3,5)
  .first((i)=>i%2===0,null,defaultValue)
  .subscribe((i)=>console.log(i));
```

Notice that we removed all even numbers from the observable, so when you run this code it will print the following:

no even numbers

## The take() operator

This operator returns an observable containing a given number of contiguous data from the observable. It has the following signature:

```
observable.take(count, [scheduler]);
```

The first parameter is mandatory and the last is optional:

- count: This is the number of elements to be retrieved from the observable
- scheduler: This is used to deliver the onCompleted() method in the case of the count equaling 0

The usage of this operator is really simple and can be seen in the following example:

```
Rx.Observable
    .of(1,3,5)
    .take(2)
    .subscribe((i)=>console.log(i));
```

In this example, we will take only the first two elements in the sequence, so running this code, we will see the following output:

1
3

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_140_0.png)

## The takeLast() operator

The takeLast() operator is analogous to the take() operator. The only difference is it takes a given count from the end of the observable. It has the following signature:

```
observable.take(count);
```

The first parameter is mandatory:

- count: This is the number of elements to be retrieved from the end of the observable

We can change the example from the previous section to use this operator:

```
Rx.Observable
.of(1,3,5)
.takeLast(2)
.subscribe((i)=>console.log(i));
```

In this code, the takeLast() operator generates a new Observable containing the last two elements in the original Observable (3 and 5), so, if you run this code, you will see the following output:

3
5

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_141_0.png)

This operator keeps a buffer with the size needed to store the requested count of elements.

## The takeWhile() operator

This is the last operator from the take() family we will see in this chapter. This operator lets you create an observable from the first contiguous sequence of elements in an observable satisfying a given conditional function. It has the following signature:

```
observable.takeWhile(condition,[context]);
```

The first parameter is mandatory and the last is optional:

- condition: This is a given function which the elements must satisfy
- context: This argument is used in the conditional function

We can use this operator to take the first contiguous sequence of even numbers, as can be seen in the following example:

```
Rx.Observable
    .of(2,4,5,6)
    .takeWhile((i)=>i%2===0)
    .subscribe((i)=>console.log(i));
```

In this code, we will test for even numbers. It will test from the beginning and stop when the first element doesn't satisfy the given condition. So, as the condition satisfies the first two numbers (2 and 4), but not the third number (5), it will print only the first two:

2
4

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_143_0.png)

If the first element doesn’t satisfy the condition, it will return an empty Observable. If we change the previous example, using a function to test for odd numbers, we can see this behavior as follows:

```
Rx.Observable
.of(2,4,5,6)
.takeWhile((i)=>i%2===1)
.subscribe((i)=>console.log(i));
```

If you run this code, you will see nothing printed in the console, as the first element (2) is not an odd number.

## The skip() operator

The skip() operator returns an Observable which bypasses a given number of elements from the beginning and propagates the others until the completion of the Observable. It has the following signature:

```
observable.skip(count);
```

The only parameter is mandatory:

- count: This is the number of elements to bypass
