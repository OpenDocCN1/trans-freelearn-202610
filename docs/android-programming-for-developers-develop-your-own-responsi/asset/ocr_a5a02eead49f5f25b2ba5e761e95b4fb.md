our Subscriber class on Observable, the onNext() function will be called three times passing each String in the Array, defined previously.

Subsequently, after all the strings are consumed by the Subscriber, the onCompleted function is called to close the stream:

```
myObservable.subscribe(mySubscriber);
```

The Observable instance is responsible for managing all subscriptions, notifying all its Subscribers, and it won't begin emitting items until we subscribe to them.

Apart from using Observable.from or another creation operator, we can create Observables by calling the create method and implementing Observable.OnSubscribe<T> that explicitly calls onNext, onError, and onCompleted.

Let's create our own Observable that emits integer numbers using the create function:

```
Observable<Integer> myObservable = Observable.create(
new Observable.OnSubscribe<Integer>() {
@Override
public void call(Subscriber<? super Integer> sub) {
// Emitting Numbers
sub.onNext(10);
sub.onNext(3);
sub.onNext(9);
// Stream completed with success
sub.onCompleted();
}
}
);
```

Remember that a well-behaved Observable must attempt to call either the observer's onCompleted or onError exactly once after emitting all the items by calling the subscriber's onNext function.

Notice that the previous Observable is also classified as a cold Observable because it will only start emitting when a Subscriber entity subscribes to it.

Alternatively, we can subscribe to observable using Action functions to handle the items dispatched in different separated functions. All that you need to do is