We can catch errors in our code, but because the code was badly implemented, we should rather fix it. Errors are not designed to be caught, but we can throw them if a critical situation occurs. A Dart program should usually terminate when an error occurs.

# Exceptions

**Exceptions**, unlike errors, are meant to be caught and usually carry information about the failure, but they don't include the stack trace information. Exceptions happen in recoverable situations and don't stop the execution of a program. You can throw any non-null object as an exception, but it is better to create a new exception class that implements the abstract class `Exception` and overrides the `toString` method of the `Object` class in order to deliver additional information. An exception should be handled in a catch clause or made to propagate outwards. The following is an example of code without the use of exceptions:

```
import 'dart:io';

main() {
  // File URI
  Uri uri = new Uri.file("test.json");
  // Check uri
  if (uri != null) {
    // Create the file
    File file = new File.fromUri(uri);
    // Check whether file exists
    if (file.existsSync()) {
      // Open file
      RandomAccessFile random = file.openSync();
      // Check random
      if (random != null) {
        // Read file
        List<int> notReadyContent =
          random.readSync(random.lengthSync());
        // Check not ready content
        if (notReadyContent != null) {
          // Convert to String
          String content = new
            String.fromCharCodes(notReadyContent);
          // Print results
          print('File content: ${content}');
        }
        // Close file
        random.closeSync();
      }
    }
  }
}
```