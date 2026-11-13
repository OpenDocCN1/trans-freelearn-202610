# Static methods

We know quite a lot about classes—how to turn them into objects and use their methods and variables. But something isn't quite right. Since the very start of the book, we have been using two classes more than any other. We have repeatedly used Log and Toast to output to the logcat or the user's screen, but have not instantiated them once! How can this be?

The static methods of classes can be used without first instantiating an object of the class. We can think of this as a static method belonging to the class and all other methods belonging to an object of a class.

And as you have probably realized by now, Log and Toast both contain static methods. To be clear: Log and Toast contain static methods; they themselves are still classes.

Classes can have both static and regular methods, as well, but the regular methods would need to be used in the regular way, via an instance of the class.

Take another look at Log.i in action:

```
Log.i("info", "our message here");
```

Here, i is the method being statically accessed, and the method takes two parameters, both of the type String.

Next, we see the static method makeText of the Toast class in use:

```
Toast.makeText(this, "our message", Toast.LENGTH_SHORT).show();
```

The makeText method of the Toast class takes three arguments.

The first argument is this, which takes some explaining. We saw when talking about constructors that to explicitly refer to the member variables of the current instance of an object, we can use this.health, this.regiment, and so on.

When we use this as we do in the previous line of code, we are referring to the instance of the class itself. It is not the Toast class but the this keyword in the previous line of code is a reference to the class the method is being used from. In