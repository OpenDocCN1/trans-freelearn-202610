
To do this, we can use the `fromPromise()` built-in method. This method can turn any A+ Promise into an observable.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_75_0.png)

Promises/A+ is a specification of promises. All the famous promises libraries in JavaScript follow this pattern; it is the same pattern followed by the standard ES6 promise.

This method has the following signature:

```
Rx.Observable.fromPromise(promise);
```

It receives only one parameter:

- `promise`: This is a Promises/A+ object or a factory function that returns an A+ Promise object.

To create an observable from a promise, use the following code:

```
Rx.Observable
.fromPromise(Promise.resolve('Hello World'))
.subscribe((result)=> console.log(result));
```

This prints the following output:

```
Hello World
```

We can also use this method to create an observable from a factory function returning a promise, as you can see in the following code:

```
var promiseFactory = () => Promise.resolve('Hello World')
Rx.Observable
.fromPromise(promiseFactory)
.subscribe((result)=> console.log(result));
```

When you run this code, you will see the following output:

```
Hello World
```

## Creating empty observables

Sometimes, when composing multiple observables, you might need to create an empty observable. You can do this in RxJS using two different methods. The first one is the `empty()` method; this method will only fire an `onComplete` event.

The following code creates an observable that finishes and doesn't propagate any value:

```
Rx.Observable.empty();
```

Sometimes you might need an observable that wouldn't emit any value and would never terminate. To create this kind of observable, you can use the never() method:

```
Rx.Observable.never();
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_76_0.png)

These two methods are used more for composition or as a fallback to prevent the absence of an observable. The only difference between the two is that only the first one terminates.

## Creating an observable from a single value

When composing multiple observables or mocking observables for testing purposes, you might need to create an observable that would emit only one value and then terminate itself. RxJS has two methods to implement this behavior: the return() method and the just() method. They work exactly in the same way and have the following signature:

```
Rx.Observable.return(value, [scheduler]);
Rx.Observable.just(value, [scheduler]);
```

The first argument is mandatory and the second is optional:

- value: This can be any object; it is the value to be emitted on the sequence
- scheduler: This is used to emit the value

The following code illustrates an example of this method:

```
Rx.Observable
  .just('Hello World')
  .subscribe((i)=> console.log(i));
```

This code uses the just() method, but we can also use the return() method with a simple change in it:

```
Rx.Observable
  .return('Hello World')
  .subscribe((i)=> console.log(i));
```

If you run any of these code lines, you will see the Hello World message printed in your console.

## Creating an observable from a factory function

There is a method in RxJS where you can create an observable from a function that returns an observable or an observable factory function; it is called defer(). The signature of this method is as follows:

```
Rx.Observable.defer(factoryFunction);
```

It receives only one parameter, and this parameter is mandatory:

- factoryFunction: This is a function that returns an observable

The following code shows an implementation of this method:

```
Rx.Observable
  .defer(function(){
    return Rx.Observable.just('Hello World');
  })
  .subscribe((data)=>console.log(data));
```

If you run this code, you will see the following output:

```
Hello World
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_77_0.png)

As the name implies, the defer() method only calls the factory function when an Observer subscribes to it.

## Creating an observable from arbitrary arguments

You have already learned how to create an observable from a sequence using the from() method, but there is a way to create an observable from an arbitrary sequence of arguments. We can do this using the of() method.

This method creates an observable that emits each argument passed to it.

The of() method has the following signature:

```
Rx.Observable.of(...args);
```

The three dots before args illustrate an arbitrary number of arguments.

- …args: This refers to any number of objects

The following code shows an implementation of the of() method:

```
Rx.Observable
  .of(0,1,2)
  .subscribe((i)=>console.log(i));
```

If you run this code, you will see the following output:

```
0
1
2
```

Now let's see what happens if we use the of() method with only one parameter on it.

Say you run the following code:

```
Rx.Observable
  .of('Hello World')
  .subscribe((i)=>console.log(i));
```

When you do this, you will see the following output:

```
Hello World
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_78_0.png)

As you can see, if you run the of() method with only one argument, it works the same way as the just() method.

Now you might be wondering what happens if you run the of() method without any argument. To check this, let's run the following code:

```
Rx.Observable
  .of()
  .subscribe((i)=>console.log(i));
```

If you run the preceding code, your program will finish without any output in the console. So if you run the of() method without any argument, it will have the same behavior as the empty() method.

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_79_0.png)

Remember that the empty() method creates an observable without any element and ends it, while the never() method creates an empty observable that never ends. So creating an observable using the of() method without any argument is the same as creating an observable using the empty() method.

## Creating an observable from an error

We can use an observable to wrap an error; this way, we will have all the operators and compositions of the concerned observable.

RxJS gives us two methods to wrap an error. Both work in exactly the same way and are basically aliases of each other.

The methods to wrap an error to an observable are throw() and throwError(). We also have a method called throwException(), but this method is deprecated and should not be used anymore.

Both methods have the same signature, as follows:

```
Rx.Observable.throw(err, [scheduler]);
Rx.Observable.throwError(err, [scheduler]);
```

The first method is the error you want to wrap around the observable and it is mandatory; the second is optional:

- err: This is any object representing an error
- scheduler: This is used to generate the sequence of the observable

Lets see a code snippet that uses the throw() method:

```
Rx.Observable
    .throw(new Error('AN ERROR HAPPENED'))
    .subscribe((data)=>console.log(data));
```

If you run the preceding code, you will see an error stack trace on your console and your program will finish.

Now let's introduce a minor change in our code.

The subscribe() method from RxJS can receive a second function to react to errors, so let's add another function to print the message of the error:

```
Rx.Observable
    .throw(new Error('AN ERROR HAPPENED'))
    .subscribe(
    (data)=>console.log(data),
    (err)=>{
        console.log('Running the subscription function for error');
        console.log(err.message)
    }
);
```

If you run the code, you will see the following error message in your console:

```
Running the subscription function for error
AN ERROR HAPPENED
```

![](img/8a5c36d94dc308a2a0fcf45ffa9cc27f_80_0.png)

Later in this chapter, we'll see the possible parameter for the subscribe() method.

In our example, we passed an Error object for the throw() method, but what happens when we pass another type of object, such as a number? To answer this question, let's introduce a minor change in our code:

```
Rx.Observable
    .throw(1)
    .subscribe(
    (data)=>console.log(data),
    (err)=>{
        console.log('Running the subscription function for error');
        console.log(err.message)
    }
);
```

If you run the preceding code, you will see the following output:

```
Running the subscription function for error
undefined
```

So as you can see, it doesn't matter what is the object. You can use the throw() method to wrap any object as an error. Using another object to represent an error is an anti-pattern, so be careful and always wrap your errors inside an Error object.
