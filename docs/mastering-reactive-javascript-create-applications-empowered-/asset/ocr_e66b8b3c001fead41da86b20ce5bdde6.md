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

Using a subject, we can easily push data into the stream. This subject is the same used throughout this chapter to push errors The only changes occur in the last line, where we request five items for our controlled() observable.

If you run this code, you will see the following output:

```
0
1
2
An error happened :(
```

Looking at the output, we see that the controlled() observable will pull the data (and run the observer) before the error happens and then propagates the error.

# More ways to filter data

In RxJS, we also have other operators to filter data. In this section, we will learn about the following operators:

+   - first()
+   - take()