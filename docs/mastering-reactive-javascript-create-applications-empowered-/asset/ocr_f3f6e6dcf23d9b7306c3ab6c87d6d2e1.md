In this code, the onErrorResumeNext () operator will create an Observable that executes the first Observable and the second when the first fails; thus, when you run this code you will see the Hello message printed in your console. However, with this example we can not really distinguish it from the catch () operator, so let's try a different example: we'll create three Observable, one of these Observables will propagate a value and then fail, and the other two will propagate a value each and completes with success. All three Observable can be seen in the following code:

```
var observableWithError = Rx.Observable.create(function(source) {
    source.onNext(1);

    source.onError(new Error('An error occurred'));

    source.onNext(2);
    source.onCompleted();
});
var helloObservable = Rx.Observable.just('Hello');
var helloAgainObservable = Rx.Observable.just('Hello again');
```

Now, in the same program, let's use the onErrorResumeNext () operator and the catch () operator with the three Observable to compare the output:

```
Rx.Observable.onErrorResumeNext(
    observableWithError,
    helloObservable,
    helloAgainObservable
).subscribe(
    (m)=> console.log('onErrorResumeNext operator propagated ===> ' + m),
    (e)=> console.log('Error found')
);
```

```
Rx.Observable.catch(
    observableWithError,
    helloObservable,
    helloAgainObservable
).subscribe(
    (m)=> console.log('catch operator propagated===> '+m),
    (e)=> console.log('Error found')
);
```