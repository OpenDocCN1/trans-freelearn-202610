```
```dart
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
```

We added `Trailer` as a mixin to `PassengerCar` and, as a result, `PassengerCar` can now carry more weight. Note that we haven't changed `PassengerCar` itself, we've only extended its functionality. At the same time, `Trailer` can be used in conjunction with the `Truck` or `Bus` classes. A mixin looks like an interface and is implicitly defined via a class declaration, but has the following restrictions:

+   - It has no declared constructor
+   - The superclass of a mixin can only be an Object
+   - They do not contain calls to `super`

# Well-designed classes

What is the difference between well-designed and poorly-designed classes?
Here are the features of a well-designed class:

+   - It hides all its implementation details
+   - It separates its interface from its implementation through the use of abstract classes
+   - It communicates with other classes only through their interfaces