
We have only one instance of a Configuration class at a point of time and that's the main purpose of the singleton pattern. One disadvantage here is the time of initialization of the configuration variable. This only happens when our program starts. It is better to use the Lazy initialization when it calls the configuration variable for the first time. The following factory constructor comes in handy here:

```dart
library configuration;

// Class configuration
class Configuration {
  // It always keeps our [Configuration]
  static Configuration _configuration;

  // Factory constructor
  factory Configuration() {
    if (_configuration == null) {
      _configuration = new Configuration._();
    }
    return _configuration;
  }

  // Database name
  String dbName;

  // Private default constructor
  Configuration._();
}
```

For now, when we refer to the Configuration class for the first time, Dart will call the factory constructor. It will check whether the private variable configuration was initialized before, create a new instance of the Configuration class if necessary, and only then return an instance of our class. The following are the changes in the framework of the code:

```dart
import 'factory_configuration.dart';

main() {
  // Access to database name
  new Configuration().dbName = 'Oracle';
  // ...
  print('Database name is ${new Configuration().dbName}');
}
```

We always get the same instance of a Configuration class when we call the factory method in this solution. Factory constructors can be widely used in the implementation of the flyweight pattern and object pooling.

## A constant constructor

Let's assume we have the following Request class in our imaginary framework:

```dart
library request;

// Request class
class Request {
  static const int AWAIT = 0;
  static const int IN_PROGRESS = 1;
  static const int SUCCESS = 2;
  static const int FAULT = 3;

  // Result of request
  int result = AWAIT;

  // Send request with optional [status]
  void send({status: IN_PROGRESS}) {
    // ...
  }
  // ...
}
```

The result variable keeps the status of the last request as an integer value. The Request class has constants to keep all the possible values of the status in one place so that we can always refer to them. This makes the code better in readability and safer too. This technique is called the enumerated pattern and is widely used. However, it has a problem when it comes to safety as any other integer value could be assigned to the result variable. This problem makes a class very fragile. Enumerated types would help us to solve this problem, but unfortunately they do not exist in Dart. The solution to this is that we create an enumerated type ourselves with the help of constant constructors as it can be used to create a compile-time constant.

We can create an abstract Enum class with the entered parameter, as follows:

```dart
library enumerated_type;

// Enum class
abstract class Enum<T> {
  // The value
  final T value;

  // Create new instance of [T] with [value]
  const Enum(this.value);

  // Print out enum info
  String toString() {
    return "${runtimeType.toString()}." +
        "${value == null ? 'null' : value.toString()}";
  }
}
```

I intentionally used a generic class in the preceding code as I don't know which type of enumeration we will create. For example, to create an enumerated type of RequestStatus based on the integer values, we can create a concrete class as follows:

```dart
import 'enum.dart';

// Enumerated type Status of Request
class RequestStatus extends Enum<int> {

  static const RequestStatus AWAIT = const RequestStatus(0);
  static const RequestStatus IN_PROGRESS = const RequestStatus(1);
  static const RequestStatus SUCCESS = const RequestStatus(2);
  static const RequestStatus FAULT = const RequestStatus(3);

  const RequestStatus(int value) : super(value);
}
```

The RequestStatus class extends the Enum class and defines the request statuses as static constant members of the class. To instantiate the RequestStatus class object, we use const instead of the new instantiation expression.

> A constant constructor creates compile-time immutable instances of a class.

Let's go back to the Request class and modify it with RequestStatus, as shown in the following code:

```dart
library request;

import 'request_status.dart';

// Request class with enum
class Request {
  // Result of request
  var result = RequestStatus.AWAIT;

  // Send request with optional [status]
  void send({status: RequestStatus.IN_PROGRESS}) {
    // ...
  }
}
```

In the preceding code, we used the enumerated type across the whole class. Finally, here is the main method in which we use the Request class:

```dart
import 'request_with_enum.dart';
import 'request_status.dart';

void main() {
  Request request = new Request();
  // ...
  request.send(status: RequestStatus.SUCCESS);
  // ...
  RequestStatus status = request.result;
  //
  switch (status) {
    case RequestStatus.AWAIT:
      print('Result is $status');
      // ...
      break;
  }
}
```

As you can see, the compile-time constants have a wide variety of uses such as default values of variables and constants, default values in method signatures, switch cases, annotations, and enumerators. Moreover, the code uses them to have a better performance and translates them into optimized JavaScript code.

Use cases of the constant constructor have the following restrictions:

- A constant constructor doesn't have a body to prevent any changes of the class state
- All the variables in a class that have a constant constructor must be final as their binding is fixed upon initialization
- The variables from the initialization list of a constant constructor must be initialized with compile-time constants

## Initializing variables

Variables reflect the state of a class instance. There are two types of variables, namely, class and instance variables. Class variables are static in Dart. The static variables of a class share information between all instances of the same class. A class provides instance variables when each instance of a class should maintain information separately from others. As we mentioned, the main purpose of a constructor is to initialize the instance of a class in a safe manner. In other words, we initialize instance variables. Let's discuss when and how we should initialize them.

Uninitialized variables in Dart have the value null so we should initialize them before using them. The initialization of variables may happen in several places. They are as follows:

- We can assign any value to a variable at the place of declaration
- A variable can be initialized in the body of a constructor
- A variable can be initialized via a constructor parameter
- Initialization can happen in the initialization list of a constructor

Where is the best place to initialize the variables? Noninitialized variables generate null reference runtime exceptions when we try to use them, as shown in the following code:

```dart
class First {
  bool isActive;

  doSomething() {
    if (isActive) {
      // ...
    }
  }
}

void main() {
  First first = new First();
  first.doSomething();
}
```

In the following code, we initialize the `isActive` variable at the place of declaration:

```dart
class First {
  final bool isActive = false;

  First();
}
```

In the Second class, we initialize the `isActive` variable via a parameter of the constructor, as follows:

```dart
class Second {
  final bool isActive;

  Second(this.isActive);
}
```

If the `isActive` final variable is indirectly dependent on the parameter of the constructor, we use the initializer list, as shown in the following code, as Dart does not allow us to initialize the final variable in a body of the constructor:

```dart
class Third {
  final bool isActive;

  Third(value) :
    this.isActive = value != null;
}
```

> The last place to initialize the final variables is in the constructor initializer list.

## Syntactic sugar

We talked a lot about the usability of the code, but I could not resist the desire to mention a couple of things about **syntactic sugar** — a syntax that is designed to make a code easier to read or express.
