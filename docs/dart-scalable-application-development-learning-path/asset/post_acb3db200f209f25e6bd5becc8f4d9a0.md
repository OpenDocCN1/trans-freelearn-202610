
## Passing a function as an argument to another function

Passing functions as arguments to other functions can be very useful in cases when we need to implement the strategy design pattern to enable the program code to be selected and executed at runtime, as shown in the following code:

```
library function_param;

// Returns sum of [a] and [b]
add(a, b) {
  return a + b;
}

// Operation executor
executor(operation, x, y) {
  return operation(x, y);
}

void main() {
  // Execute operation
  var result = executor(add, 2, 1);
  print("Result is ${result}");
}
```

Here is the result of the preceding code:

```
Result is 3
```

The global executor function from the preceding example can call any function that accepts two arguments. You can see the implementation of the strategy design pattern in the form of anonymous functions passed as parameters of methods in collections.

## Returning a function as a result of another function

Sometimes, a function can be returned as a result of another function, as shown in the following code:

```
library function_return;

// Returns sum of [a] and [b]
```

Here is the result of the preceding code:

```
Result is 3
```

We have two functions and the array **operations** in our example that stores references to them.

## Closures

A function can be created in the global scope or within the scope of another function. A function that can be referenced with an access to the variables in its lexical scope is called a closure, as shown in the following code:

```
library function_closure;

// Function returns closure function.
calculate(base) {
  // Counter store
  var count = 1;
  // Inner function - closure
  return () => print("Value is ${base + count++}");
}

void main() {
  // The outer function returns inner
  var f = calculate(2);
  // Now we call closure
  f();
  f();
}
```

Here is the result of the preceding code:

```
Value is 3
Value is 4
```

We have the `calculate` function, which contains the `count` variable and returns an inner function. The inner function has an access to the `count` variable because both are defined in the same scope. The `count` variable exists only within the scope of `calculate` and would normally disappear when the function exits. This does not happen in this case because the inner function returned by `calculate` holds a reference to `count`. The variable has been closed over, meaning it's within a closure.

Finally, we know what a **first-class function** is, where we can use them, and how important it is to use closures. Let's move ahead to classes and mixins.

## Classes and mixins

We all know it's wasteful trying to reinvent the wheel. It's even more wasteful trying to do it each time we want to build a car. So how can a program code be written more efficiently and made reusable to help us develop more powerful applications? In most cases, we turn to the OOP paradigm when trying to answer this question. OOP represents the concept of objects with data fields and methods that act on that data. Programs are designed to use objects as instances of classes that interact with each other to organize functionality.

### Types

The Dart language is dynamically typed, so we can write programs with or without the type annotations in our code. It's better to use the type annotations for the following reasons:

- The type annotations enable early error detection. The static analyzer can warn us about the potential problems at the points where you've made the mistakes.
- Dart automatically converts the type annotations into runtime assertion checks. In the checked mode, the dynamic type assertions are enabled and it can catch some errors when types do not match.
- The type annotations can improve the performance of the code compiled in JavaScript.
- They can improve the documentation, making it much easier to read the code.
- They can be useful in special tools and IDE such as the name completion.

The fact that the type annotations were not included in our code does not prevent our program from running. The variables without the type annotations have a dynamic type and are marked with var or dynamic. Here are several recommendations where the type annotations are appropriate:

- You should add types to public and private variables
- You can add types to parameters of methods and functions
- You should avoid adding types to the bodies of methods or functions

If we define a racing car, then we must implement all the characteristics and behaviors of the Car class, but with substantial changes to the engine, suspension, brakes, and so on:

```
import 'car.dart';
import 'passenger_car.dart';

void main() {
  // Create an instance of passenger car of white color,
  // carrying 750 kg and max passengers 5.
  Car car = new PassengerCar('white', 750.0, 5);
  // Move it
  car.move(100.0);
}
```

Here, we just created an instance of PassengerCar and assigned it to the car variable without defining any special interfaces.

### Mixins

Dart has a mixin-based inheritance, so the class body can be reused in multiple class hierarchies, as shown in the following code:

```
library trailer;

// The trailer
class Trailer {
  // Access to car's [carrying] info
  double carrying = 0.0;

  // Trailer can carry [weight]
  void carry(double weight) {
    // Car's carrying increases on extra weight.
    carrying += weight;
  }
}
```

The Trailer class is independent of the Car class, but can increase the carrying weight capacity of the car. We use the with keyword followed by the Trailer class to add mixin to the PassengerCar class in the following code:

```
library passenger_car;

import 'car.dart';
import 'trailer.dart';

// Passenger car with trailer.
class PassengerCar extends Car with Trailer {
  // Max number of passengers.
  int maxPassengers = 4;

  /**
   * Create [PassengerCar] with [color], [carrying] and [maxPassengers].
   * We can use [Trailer] to carry [extraWeight].
   */
  PassengerCar(String color, double carrying, this.maxPassengers,
      {double extraWeight: 0.0}) : super(color, carrying) {
    // We can carry extra weight with [Trailer]
    carry(extraWeight);
  }
}
```

We added `Trailer` as a mixin to `PassengerCar` and, as a result, `PassengerCar` can now carry more weight. Note that we haven't changed `PassengerCar` itself, we've only extended its functionality. At the same time, `Trailer` can be used in conjunction with the `Truck` or `Bus` classes. A mixin looks like an interface and is implicitly defined via a class declaration, but has the following restrictions:

- It has no declared constructor
- The superclass of a mixin can only be an Object
- They do not contain calls to `super`

### Well-designed classes

What is the difference between well-designed and poorly-designed classes?
Here are the features of a well-designed class:

- It hides all its implementation details
- It separates its interface from its implementation through the use of abstract classes
- It communicates with other classes only through their interfaces

All the preceding properties lead to encapsulation. It plays a significant role in OOP. Encapsulation has the following benefits:

- Classes can be developed, tested, modified, and used independently
- Programs can be quickly developed because classes can be developed in parallel
- Class optimization can be done without affecting other classes
- Classes can be reused more often because they aren't tightly coupled
- Success in the development of each class leads to the success of the application

All our preceding examples include public members. Is that right? So what is the rule that we must follow to create well-designed classes?

### To be private or not

Let's follow the simple principles to create a well-designed class:

- Define a minimal public API for the class. Private members of a class are always accessible inside the library scope so don't hesitate to use them.
- It is not acceptable to change the level of privacy of the member variables from private to public to facilitate testing.
- Nonfinal instance variables should never be public; otherwise, we give up the ability to limit the values that can be stored in the variable and enforce invariants involving the variable.
- The final instance variable or static constant should never be public when referring to a mutable object; otherwise, we restrict the ability to take any action when the final variable is modified.
- It is not acceptable to have a public, static final instance of a collection; otherwise, the getter method returns it, and we restrict the ability to modify the content of the collection.

The last two principles can be seen in the following example. Let's assume we have a Car class with a defined final static list of parts. We can initialize them with Pedal and Wheel, as shown in the following code:

```
class Car {
  // Be careful with that code !!!
  static final List PARTS = ['Pedal', 'Wheel'];
}
void main() {
  print('${Car.PARTS}'); // Print: [Pedal, Wheel]
```
