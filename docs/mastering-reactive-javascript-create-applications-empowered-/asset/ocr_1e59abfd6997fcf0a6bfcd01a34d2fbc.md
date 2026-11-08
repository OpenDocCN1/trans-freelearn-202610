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
    )
```