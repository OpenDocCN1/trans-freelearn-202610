![](bbox=[0.07, 0.11, 0.16, 0.17])

As you can see, it never prints Now the stream will finish because the error happened before it.

# Properties

Now you know what an EventStream is and how to create it; as discussed, it is an observable. However, bacon.js has another special form of observable called Property. A Property is an EventStream with the concept of current value. You can create a Property from any EventStream using the toProperty() or scan() method; it is especially useful to represent DOM data binding.

To create a Property from an EventStream, you can use the toProperty() method that has the following signature:

```
eventStream.toProperty(initialValue);
```

The initialValue parameter is optional. If you decide to omit it, you will have a Property without an initial value. If you pass it, it will be used as the current value of this Property until the first value is emitted from the stream.

The other way to create a Property is using the scan() method. This method is similar to the reduce() method from the Javascript array object. Given a seed object and accumulator function, the scan() method will iterate over your EventStream to create a Property from the result:

```
Bacon
.sequentially(100, ['a','b','c','d'])
.scan('=> ', (acc,b)=> acc+b)
.onValue((value)=>console.log(value));
```

In this example, we first create an EventStream from an array of strings, then we call the scan() method to create a Property with a seed value of => , and using a function to concatenate these value, we print the result to the console for each scan iteration. So running this code will give the following output:

```
=>
=> a
=> ab
=> abc
=> abcd
```