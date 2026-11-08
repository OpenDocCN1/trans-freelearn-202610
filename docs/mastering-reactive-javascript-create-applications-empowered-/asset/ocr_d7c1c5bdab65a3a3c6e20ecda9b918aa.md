The three dots before args illustrate an arbitrary number of arguments.

+   - …args: This refers to any number of objects

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

![](bbox=[0.11, 0.57, 0.2, 0.64])

As you can see, if you run the of() method with only one argument, it works the same way as the just() method.

Now you might be wondering what happens if you run the of() method without any argument. To check this, let's run the following code:

```
Rx.Observable
  .of()
  .subscribe((i)=>console.log(i));
```

If you run the preceding code, your program will finish without any output in the console. So if you run the of() method without any argument, it will have the same behavior as the empty() method.