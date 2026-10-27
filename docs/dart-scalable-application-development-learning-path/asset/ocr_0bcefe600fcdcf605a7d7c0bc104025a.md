The factory constructor of the Log class is a static method and every time it creates a new instance of ConsoleLog. The updated version of the Adventure class looks like the following code:

```
import 'factory_log.dart';

// Adventure class with log
class Adventure {
  static Log log = new Log();

  walkMethod() {
    log.debug("entering log");
    // ...
  }
}
```

Now, we are not referring to ConsoleLog as an implementation of Log, but only using the factory constructor. All the changes from ConsoleLog to FileLog will happen in one place, that is, inside the factory constructor. However, it would be nice to use different implementations that are appropriate in specific scenarios without altering the Log class as well. This can be done by adding a conditional statement in the factory constructor and instantiating different subclasses, as follows:

```
library log;

part 'factory_console_log.dart';
part 'factory_file_log.dart';

// Log information
abstract class Log {

  static bool useConsoleLog = false;

  factory Log() {
    return useConsoleLog ?
      new ConsoleLog() :
      new FileLog();
  }

  debug(String message);
  // ...
}
```