
The easiest way to subscribe to an observable is to just pass the onNext parameter (as we have been doing in most of our code snippets in this chapter):

```
Rx.Observable
.just('Hello World!!!')
.subscribe((message)=>console.log(message));
```

If you run the preceding code, it will print the Hello World!!! message in your console.

We can also add an onError function to be called when an error happens:

```
Rx.Observable
.throw(new Error('ops'))
.subscribe(
    (message)=>console.log(message),
    (err)=>console.log('An error happened: '+err.message)
);
```

If you run this code, you will see the following output in your console:

```
An error happened: ops
```

Lastly, we can also add the onCompleted() function to take an action when the observable is completed:

```
Rx.Observable
.just('Hello World!!!')
.subscribe(
    (message)=>console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);
```

If you run this code, you will see the following output:

```
Hello World!!!
END
```

All subscriptions have a method called dispose(). This method can be used to stop listening to incoming data in the observable. We can test it in the following code:

```
var subscription = Rx.Observable
    .interval(100)
    .subscribe(
        (message)=>console.log(message),
        (err)=>console.log('An error happened: '+err.message),
        ()=>console.log('END')
    );

setTimeout( () => subscription.dispose(), 290 );
```

As illustrated in the code, we first create an observable that would emit data every 100 milliseconds and we subscribe to it. Then we use the `setTimeout()` function to call the `dispose()` method of this subscription and stop listening to data from the original observable.

If you run this code, you will see the following output in your console:

```
0
1
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_88_0.png)

Remember, the `interval()` method will wait for 100 milliseconds before sending the number 0. Then, it will wait for 100 milliseconds more to send the number 1.

In RxJS, we also have a special class of objects called Observer. Basically, this object wraps the functions `onNext()`, `onError()`, and `onCompleted()` that you use to subscribe to an observable, giving you an extra layer of abstraction.

To create an observer, we can use the `create()` method and then use this observer to subscribe to an observable, as you can see in following code:

```
var observer = Rx.Observer.create(
    (message) => console.log(message),
    (err) => console.log('An error happened: ' + err.message),
    () => console.log('END')
);

Rx.Observable
    .just('Hello World!!!')
    .subscribe(observer);
```

If you run this code, you will see the following output:

```
Hello World!!!
END
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_88_1.png)

Observer is the object responsible for reacting to data sent by an Observable.

## RxJS Subjects

Subjects could be both an Observable and an Observer. They can be seen as a pushable Observable; they let you add more data to be propagated through them.

Subjects expose three important methods: onNext(), onError(), and onCompleted(). These methods can be used to send events through the observable sequence. You can see their usage in the following example:

```js
var subject = new Rx.Subject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext('Hello World!!!');
subject.onCompleted();
```

In this example, we created a new subject and subscribed to it, using the subscribe() method. Then, we pushed data on this subject using the onNext() method, and we finally finished it calling the onCompleted() method.

If you run this code, you will see the following output:

```
Hello World!!!
END
```

We can also propagate an error through a subject using its onError() method:

```js
var subject = new Rx.Subject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext('Hello World!!!');
subject.onError(new Error('ops'));
```

First, we propagate the value Hello World, which will be printed in the console. Then, we propagate an error. If you run the preceding code, you will see the following output in your console:

```
Hello World!!!
An error happened: ops
```

There is another type of subject called AsyncSubject (). It can be used to represent the result of an asynchronous operation. An AsyncSubject () can receive only one value, and this value is then cached for all future subscriptions of this subject.

An example of AsyncSubject () creation can be seen in the following code:

```
var subject = new Rx.AsyncSubject();

subject.subscribe(
  (message)=> console.log(message),
  (err)=>console.log('An error happened: '+err.message),
  ()=>console.log('END')
);

subject.onNext('Hello World!!!');
subject.onCompleted();
```

As you can see, the only difference to create it–when compared to a Subject–appears on the first line of code, where we use new Rx.AsyncSubject () instead of new Rx.Subject (). If you run this code, you will see the following output in your console:

```
Hello World!!!
END
```

As discussed earlier, an AsyncSubject () must be used to propagate only one piece of data. So let's compare it with a regular Subject.

First, let's see what the output of a Subject would be when we call the onNext () method multiple times:

```
var subject = new Rx.Subject();

subject.subscribe(
  (message)=> console.log(message),
  (err)=>console.log('An error happened: '+err.message),
  ()=>console.log('END')
);

subject.onNext(0);
subject.onNext(1);
subject.onNext(2);
subject.onCompleted();
```

If you run this code, you will have the following content printed in the console:

```
0
1
2
END
```

This shows us that the Observer was fired three times (for elements 0, 1, and 2). Now let's change our code to use an AsyncSubject():

```
var subject = new Rx.AsyncSubject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext(0);
subject.onNext(1);
subject.onNext(2);
subject.onCompleted();
```

If we look at the console after running this code, we will see the following printed:

```
2
END
```

As expected, an AsyncSubject() propagates only one element, and for this reason, it fires the Observer once (with two elements).

Lastly, let's see what happens if we don't call the onCompleted() method in our Subjects.

First, let's use a regular subject:

```
var subject = new Rx.Subject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext (0);
subject.onNext (1);
subject.onNext (2);
```

If we run this code, we will see the following output:

0
1
2

We can see that a Subject doesn't wait for the onCompleted() method to be called to propagate the elements.

Now let's check out the output of AsyncSubject():

```
var subject = new Rx.AsyncSubject();

subject.subscribe(
    (message)=> console.log(message),
    (err)=>console.log('An error happened: '+err.message),
    ()=>console.log('END')
);

subject.onNext (0);
subject.onNext (1);
subject.onNext (2);
```

If we run this code, nothing is printed in the console, so an AsyncSubject() will wait for the onCompleted() method before it can start propagating the element. So it's easy to see that when dealing with AsyncSubject(), we must always call the onNext() and onCompleted() methods together.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_92_0.png)

The easiest way to understand a Subject is to see it as a pushable observable.

## RxJS Disposable

The Disposable class gives us a method to release allocated resources (database connections, file handlers, and so on). We can do this by calling the dispose() method of this object.
