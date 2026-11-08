We can use this method to avoid marking readStream as an utf8 string, and we can do this only in the observable. To do this, use the following code:

```
var fs = require('fs');
var Rx = require('rx');

var readStream = fs.createReadStream(__filename);
Rx.Observable
    .fromEvent(readStream,'data',(chunk)=>chunk.toString('utf8'))
    .subscribe((i)=> console.log(i));
```

# Creating an observable from an arbitrary source

In the previous chapter, we discussed how to implement an observable from an arbitrary source in bacon.js, using the fromBinder event. On RxJS, we can also create an observable from any source that we want using the create() method. There is also an alias for this method, called createWithDisposable.

The create() method has the following signature:

```
Rx.Observable.create(sourceFunction);
```

This method accepts only one parameter and it is mandatory:

+   - sourceFunction: This is a function that receives an object capable of pushing data into an observable

We can create an observable with the create() method using the following code:

```
Rx.Observable.create(function(source){
    source.onNext(0);
    source.onNext(1);
    source.onNext(2);
    source.onCompleted();
}).subscribe((i)=> console.log(i));
```