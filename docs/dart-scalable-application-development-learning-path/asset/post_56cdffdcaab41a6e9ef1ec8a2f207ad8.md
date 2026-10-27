
```dart
// Change part
Car.PARTS.remove('Wheel');
print('${Car.PARTS}'); // Print: [Pedal]
}
```

However, there's a problem here. While we can't change the actual collection variable because it's marked as final, we can still change its contents. To prevent anyone from changing the contents of the collection, we change it from final to constant, as shown in the following code:

```dart
class Car {
  // This code is safe
  static const List PARTS = const ['Pedal', 'Wheel'];
}

void main() {
  print('${Car.PARTS}'); // Print: [Pedal, Wheel]

  // Change part
  Car.PARTS.remove('Wheel');
  print('${Car.PARTS}');
}
```

This code will generate the following exception if we try to change the contents of PARTS:

```text
Unhandled exception:
Unsupported operation: Cannot modify an immutable array
#0 List.remove (dart:core-patch/array.dart:327)
...
```

## Variables versus the accessor methods

In the previous section, we mentioned that nonfinal instance variables should never be public, but is this always right? Here's a situation where a class in our package has a public variable. In our Car class, we have a color field and it is deliberately kept as public, as shown in the following code:

```dart
// Is that class correct?
class Car {
  // Color of the car.
  String color;
}
```

If the Car class is accessible only inside the library, then there is nothing wrong with it having public fields, because they don't break the encapsulation concept of the library.

## Inheritance versus composition

We defined the main rules to follow and create a well-designed class. Everything is perfect and we didn't break any rules. Now, it's time to use a well-designed class in our project. First, we will create a new class that extends the current one. However, that could be a problem as inheritance can break encapsulation.

It is always best to use inheritance in the following cases:

- Inside the library, because we control the implementation and relationship between classes
- If the class was specifically designed and documented to be extended

It's better not to use inheritance from ordinary classes because it's dangerous. Let's discuss why. For instance, someone developed the following Engine class to start and stop the general purpose engine:

```dart
// General purpose Engine
class Engine {
  // Start engine
  void start() {
    // ...
  }

  // Stop engine
  void stop() {
    // ...
  }
}
```

We inherited the DieselEngine class from the Engine class and defined when to start the engine that we need to initialize inside the init method, as shown in the following code:

```dart
import 'engine.dart';

// Diesel Engine
class DieselEngine extends Engine {
  DieselEngine();

  // Initialize engine before start
  void init() {
    // ...
  }

  void start() {
    // Engine must be initialized before use
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

We created the private `_engine` variable in our `DieselEngine` class that references an instance of the `Engine` class. `Engine` now becomes a component of `DieselEngine`. This is called a composition. Each method in `DieselEngine` calls the corresponding method in the `Engine` instance. This technique is called forwarding, because we forward the method's call to the instance of the `Engine` class. As a result, our solution is safe and solid. If a new method is added to `Engine`, it doesn't break our implementation.

The disadvantages of this approach are associated with performance issues and increased memory usage.

## Methods and operators

Now that we've introduced well-designed classes, we need to discuss methods.

### Checking the values of the parameters before using them

The class constructors, methods, mutators (setters), and operators impose some restrictions on the values that must be passed into their parameters. What will happen if an invalid parameter value is passed to a method? One possibility is that the method will fail with a confusing exception or worse, it will succeed but with a wrong result. In any case, it's dangerous not to check the parameters of a method before using them. The rule here is to check whether the parameter value is valid as soon as possible. The best place to do that is at the beginning of the method.

The Dart VM can work in a developer-friendly checked mode and a speed-obsessed production mode. We usually use the checked mode when developing our applications. One of the benefits of this mode is the dynamic assertion. We should use the `assert` statement to check whether the parameters of the method are valid before using them. The Dart VM continues the program execution if the Boolean result of the dynamic assertion is `true`, otherwise stops it. This is shown in the following code:

```dart
/**
 * Return sum of [a] and [b].
 * It throws [AssertionError] if any of [a] or [b] equals null
 */
sum(int a, int b) {
  assert(a != null);
  assert(b != null);
  return a + b;
}
```

> The assert statement has no effect when the program executes in the production mode or is compiled to JavaScript code.

We must check the validity of the parameters before storing them for later use. Ignoring this can lead to problems later because an error associated with the parameter can be thrown in a completely different place, making it harder to trace its source. This has serious implications, especially in constructors.

Sometimes, it is important to validate the internal state of a class in the method and generate a special error, as shown in the following code. The typical errors are `StateError`, `RangeError`, and `ArgumentError`.

```dart
class Car {
  double petrol;

  /**
   * Start engine.
   * That method throws [StateError] if petrol is null
   * or less than 5 liters.
   */
  void startEngine() {
    if (petrol == null || petrol <= 5.0) {
      throw new StateError('Not enough petrol');
    }
  }
}
```

Here, we have a Car class with the `petrol` variable and the `startEngine` method. The `startEngine` method checks whether there is enough petrol to start the engine; otherwise, it throws an error.

> Each time you create a method, think about the restrictions that apply to its parameters.

### Well-designed methods

So, now that we've defined well-designed classes, it's time to define well-designed methods. We must remember that methods are part of a class' interface and the following simple rules can make them easier to use and also less error-prone:

- Choose the right method name. Remember, Dart doesn't support method overloading. Instead, we can have different method names or optional parameters.
- Use optional named parameters. This helps programmers to use your methods without the need to remember the position of each parameter.
- Refer to objects in terms of their interfaces over classes as the type of parameters. For example, we have an interface and the class implements that interface. Use the interface as the parameter type of the method instead of a concrete one. Don't restrict the solution to a particular implementation.

A car may have the following different types of engines:

```dart
// Engine interface
abstract class Engine {
  void start();
}

// Diesel engine
class DieselEngine implements Engine {
  void start() {
    // ...
  }
}

// Carburetor engine
class CarburetorEngine implements Engine {
  void start() {
    // ...
  }
}
```

```dart
// Car
class Car {
  var engine;

  // Car may have any engine
  Car(Engine this.engine);
}
```

It's better to pass the abstract Engine class as a parameter of the constructor for the car to prevent any problems in future.

> **Downloading the example code**
>
> You can download the example code files for all Packt books you have purchased from your account at `http://www.packtpub.com`. If you purchased this book elsewhere, you can visit `http://www.packtpub.com/support` and register to have the files e-mailed directly to you.

## Summary

This chapter covered some of the most useful advanced features of the Dart language. The Dart language was designed with the modules in mind. Modularity in Dart is realized through packages, libraries, and classes. The code compiled in JavaScript loses all the library information.

Functions are first-class citizens because they support all the operations generally available to other types. A function that can be referenced with an access to the variables in its lexical scope is called a closure.

Programs are designed to use objects as instances of classes that interact with each other to organize functionality. The Dart language is dynamically typed, so we can write programs with or without the type annotations in our code.

In the next chapter, we will talk about generics, errors and exceptions, and annotations and reflection.
