# # FAQ

1. What is wrong with this method definition?

```
doSomething(){
 // Do something here
}
```

No return type is declared. You do not have to return a value from a method, but its return type must be void in this case. This is how the method should look:

```
void doSomething(){
 // Do something here
}
```

2. What is wrong with this method definition?

```
float getBalance(){
 String customerName = "Linus Torvalds";
 float balance = 429.66f;
 return customerName;
}
```

The method returns a String (userName) variable, but the signature states that it must return a float. With a method name like getBalance, this code is probably what was intended:

```
float getBalance(){
 String customerName = "Linus Torvalds";
 float balance = 429.66f;
 return balance;
}
```

+   • When do we call the onCreate method? !Trick question alert!

We don't. Android decides when to call oncreate as well as all the other methods that make up the lifecycle of an Activity. We just override the ones that are useful to us. We do, however, call super.oncreate so that our overridden version and the original version both get executed.

# Note

For the sake of technical accuracy, it is possible to call the lifecycle methods