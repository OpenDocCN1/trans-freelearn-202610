```
```dart
// Change part
Car.PARTS.remove('Wheel');
print('${Car.PARTS}'); // Print: [Pedal]
}
```
```

However, there's a problem here. While we can't change the actual collection variable because it's marked as final, we can still change its contents. To prevent anyone from changing the contents of the collection, we change it from final to constant, as shown in the following code:

```
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
```

This code will generate the following exception if we try to change the contents of PARTS:

```
```text
Unhandled exception:
Unsupported operation: Cannot modify an immutable array
#0 List.remove (dart:core-patch/array.dart:327)
...
```
```

# Variables versus the accessor methods

In the previous section, we mentioned that nonfinal instance variables should never be public, but is this always right? Here's a situation where a class in our package has a public variable. In our Car class, we have a color field and it is deliberately kept as public, as shown in the following code:

```
```dart
// Is that class correct?
class Car {
  // Color of the car.
  String color;
}
```
```

If the Car class is accessible only inside the library, then there is nothing wrong with it having public fields, because they don't break the encapsulation concept of the library.