
This is illustrated in the following screenshot:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_121_0.png)

## The pausable() observables

The last operator to implement the lossy technique to deal with backpressure is the pausable() operator. This operator returns a pausable() observable so we can pause it (and discard all the data while paused) and resume it when we are ready to accept more data.

The pausable() method has the following signature:

```
observable.pausable();
```

As you can see, it doesn't accept any parameter.

We can see an example of the usage of this operator in the following code:

```
var pausableObservable = Rx.Observable
    .interval(50)
    .pausable();
pausableObservable.subscribe((i)=>console.log(i));

pausableObservable.pause();

setTimeout(()=>{
    pausableObservable.resume();
},2000);
```

If you run this code, you will see the following output in your console:

```
0
1
2
3
//continues printing until you stop the program
```

In this example, we create an Observable which emits data every 50 milliseconds, but we pause it right after the subscription, so no data is emitted. Then after 2000 milliseconds, a function is called and we resume the observable, so it starts to print the data.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_122_0.png)

This method works only with a hot observable. The observables created with the interval() method are cold observables and for this reason no data was discarded.

Now let's check this method in a subject:

```
var subject = new Rx.Subject();
var pausableObservable = subject.pausable();

pausableObservable.subscribe(
  (i)=>console.log(i),
  (err)=>console.log(err),
  ()=>console.log('Finished')
);

pausableObservable.pause();

subject.onNext(0);
subject.onNext(1);

setTimeout(()=>{
  subject.onNext(2);
  subject.onNext(3);
  subject.onCompleted();
},200);
setTimeout(()=>{
  pausableObservable.resume();
},2000);
```

We are using the same subject we used in all the examples in this session. The only change is that we pause it before we push any data and only resume it after all the data has been pushed, so if we run this code we will see this printed in the console:

```
Finished
```

All the data is discarded because the data was pushed while the observable was paused. This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_123_0.png)

## Loss-less operators to deal with backpressure

Loss-less strategies to deal with backpressure are the ones we must use when we can't afford losing data. If you are using them to show messages in a web chat, you might want to buffer them.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_123_1.png)

Be careful when using loss-less strategies to mitigate the problem of backpressure as this increases the memory your program will need.

## Buffering

The first loss-less technique to deal with backpressure is using a buffer to store the data, so we can process it in a batch.

All methods for buffering data start with buffer in the name (here we will learn bufferWithCount(), bufferWithTime(), and bufferWithTimeOrCount()), and basically they will store data in memory until the buffer is fulfilled and propagates an array containing the data.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_124_0.png)

If an error message is propagated by the observable, this message will be sent immediately without first emitting the buffer, even if the buffer contains data.

## The bufferWithCount() operator

The first method to buffer data is bufferWithCount(). The bufferWithCount() method lets you specify the size of the buffer. With this method, the data will be stored in an array until the size of this array reaches the given count, upon which this array is propagated.

The bufferWithCount() method has the following signature:

```
observable.bufferWithCount(size, [skip]);
```

The first parameter is mandatory and the second is optional:

- size: This is a number specifying the size of the buffer.
- skip: This is a number specifying how many items we should skip to create the next buffer. If not provided, it defaults to the size (this way the next buffer will contain only new elements).

Now let's see an example of this method:

```
Rx.Observable
  .interval(50)
  .bufferWithCount(2)
  .subscribe((arr)=>console.log(arr));
```

In this example, we first create an Observable from an interval of 50 milliseconds. We then use the bufferWithCount() method to buffer it in chunks of 10, and finally we print it to the console.

If you run this code, you will see the following output:

```
[ 0, 1]
[2, 3]
//continues printing until you stop the program
```

This operation is illustrated in the following diagram:

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_125_0.png)

Now let's see what happens if an error is propagated in our `buffer`. For this, let's create a `subject`:

```
var subject = new Rx.Subject();

var bufferedObservable = subject.bufferWithCount(2);

bufferedObservable.subscribe(
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
```

In this example, we first create a `subject` and use the `bufferWithCount()` to create chunks of two items and subscribe to it. We then push three values (0, 1, and 2). The first two (0 and 1) will be propagated and printed in the console, the third is buffered, until an error is pushed into the `subject`. This makes the error go directly through the observable.

If you run this code, you will see the following output:

```
[ 0, 1 ]
An error happened :(
```

Now, you might be wondering what happens if an observable finish and the count doesn't reach the desired count. To test it, we can use a simple program:

```
Rx.Observable
  .of(0,1,2,3,4)
  .bufferWithCount(2)
  .subscribe((i)=> console.log(i));
```

If you run this code, you will see the following output:

```
[ 0, 1 ]
[ 2, 3 ]
[ 4 ]
```

So, as you can see, if the original observable finishes, RxJS will flush the current buffer.

Let's see now how the second parameter works, comparing the two subscriptions:

```
var observable = Rx.Observable.interval(1000);

var observableWithDefaultSkip = observable.bufferWithCount(4);
var observableWithSkipOne = observable.bufferWithCount(4,1);

observableWithDefaultSkip.subscribe((arr)=>console.log('with default skip :'+arr));
observableWithSkipOne.subscribe(
  (arr)=>console.log('with default skip equals to one:'+arr)
);
```

If you run this code (and wait a little bit), you will see an output as follows:

```
with default skip :0,1,2,3
with default skip equals to one:0,1,2,3
with default skip equals to one:1,2,3,4
with default skip equals to one:2,3,4,5
with default skip equals to one:3,4,5,6
with default skip :4,5,6,7
with default skip equals to one:4,5,6,7
//continues printing until you stop the program
```

As you can see by the output, if you don't provide the skip value, the buffer is basically erased after being propagated. This way it runs only every 4 seconds (as our original observable pushes the value every second, it takes 4 seconds to fill four items in the array), but if you provide a skip value, it will remove this amount of items (from left to right) and always push new values in the end of the array. This way it runs every second (because our buffer always keeps three items from the last buffer, it takes only 1 second to fill the array).

And finally, one useful application of the bufferWithCount() method is to calculate the average of the last X numbers in the sequence. To show this application, let's first create an Observable to generate random numbers:

```javascript
var randomNumbersGenerator = Rx.Observable
    .interval(100)
    .map((i)=>Math.floor(Math.random()*100));
```

Here we create an Observable to generate items every 100 milliseconds. We then map it to an integer value between 0 and 99.

Now let's calculate the average of the last five items. To do this, we call the bufferWithCount() method with a size of 5 and skip 1. Then we calculate the average and print in the console.

The full code is as follows:

```javascript
var randomNumbersObservable = Rx.Observable
    .interval(100)
    .map((i)=>Math.floor(Math.random()*100));

randomNumbersObservable
    .bufferWithCount(5,1)
    .map((arr)=>{
        var sum = arr.reduce((acc,b)=>acc+b);
        return sum/arr.length;
    })
    .subscribe((i)=>console.log('The average of the last five items is: '+i));
```
