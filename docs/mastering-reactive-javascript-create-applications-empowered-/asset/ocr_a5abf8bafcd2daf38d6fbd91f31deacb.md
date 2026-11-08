If you run this code, you will see the following output in your console:

```
0
1
2
3
//continues printing until you stop the program
```

In this example, we create an Observable which emits data every 50 milliseconds, but we pause it right after the subscription, so no data is emitted. Then after 2000 milliseconds, a function is called and we resume the observable, so it starts to print the data.

![](bbox=[0.11, 0.32, 0.19, 0.4])

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