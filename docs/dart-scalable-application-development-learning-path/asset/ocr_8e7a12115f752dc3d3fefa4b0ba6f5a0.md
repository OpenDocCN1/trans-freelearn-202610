# # The factory method design pattern

Let's imagine that we are creating a framework and it's time to log the information about different operations of the class methods. The following simple Log class can work for us:

```
```dart
library log;

// Log information
abstract class Log {
  debug(String message);
  // ...
}
```
```

We can print the log information to the console with the ConsoleLog class as follows:

```
```dart
library log.console;

import 'log.dart';

// Write log to console
class ConsoleLog implements Log {
  debug(String message) {
    // ...
  }
  // ...
}
```
```

We can also save the log messages in a file with the FileLog class as follows:

```
```dart
library log.file;

import 'log.dart';

// Write log to file
class FileLog implements Log {
  debug(String message) {
    // ...
  }
  // ...
}
```
```