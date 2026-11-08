All methods for buffering data start with buffer in the name (here we will learn bufferWithCount(), bufferWithTime(), and bufferWithTimeOrCount()), and basically they will store data in memory until the buffer is fulfilled and propagates an array containing the data.

![](bbox=[0.13, 0.2, 0.2, 0.27])

If an error message is propagated by the observable, this message will be sent immediately without first emitting the buffer, even if the buffer contains data.

# The bufferWithCount() operator

The first method to buffer data is bufferWithCount(). The bufferWithCount() method lets you specify the size of the buffer. With this method, the data will be stored in an array until the size of this array reaches the given count, upon which this array is propagated.

The bufferWithCount() method has the following signature:

```
observable.bufferWithCount(size, [skip]);
```

The first parameter is mandatory and the second is optional:

+   - size: This is a number specifying the size of the buffer.
+   - skip: This is a number specifying how many items we should skip to create the next buffer. If not provided, it defaults to the size (this way the next buffer will contain only new elements).

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