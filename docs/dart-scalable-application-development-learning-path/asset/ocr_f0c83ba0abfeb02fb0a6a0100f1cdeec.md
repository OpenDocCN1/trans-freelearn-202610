```
```dart
// ...
}

void start() {
  // Engine must be initialized before use
  init();
  // Start engine
  _engine.start();
}

void stop() {
  _engine.stop();
}
}
```
```

We created the private `engine` variable in our `DieselEngine` class that references an instance of the `Engine` class. `Engine` now becomes a component of `DieselEngine`. This is called a composition. Each method in `DieselEngine` calls the corresponding method in the `Engine` instance. This technique is called forwarding, because we forward the method's call to the instance of the `Engine` class. As a result, our solution is safe and solid. If a new method is added to `Engine`, it doesn't break our implementation.

The disadvantages of this approach are associated performance issues and increased memory usage.

# # Methods and operators

Now that we've introduced well-designed classes, we need to discuss methods.

# ## Checking the values of the parameters before using them

The class constructors, methods, mutators (setters), and operators remove some restrictions on the values that must be passed into their parameters. What will happen if an invalid parameter value is passed to a method? One possibility is that the method will fail with a confusing exception or worse it will succeed but with a wrong result. In any case, it's dangerous not check the parameters of a method before using them. The rule here is to check whether the parameter value is valid as soon as possible. The best place to do that is at the beginning of the method.