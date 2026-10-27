# How to do it...

In our script, we have a list, persons, where the items are themselves lists consisting of a name and a gender. We want to walk through the list and emit a greeting message based on the gender of the person, but if the gender is unknown, we skip that person. The following code shows us how we can do this with a transformer:

```
```dart
import 'dart:async';

var persons = [
  ['Carter', 'F'],
  ['Gates', 'M'],
  ['Nuryev', 'M'],
  ['Liszt', 'U'],
  ['Besançon', 'F']
];

void main() {
```
```

We need to perform the following steps to transform the streams:

+   1. To make a stream from the list, use the following code:
+   
+      ```dart
+      var stream = new Stream.fromIterable(persons);
+      ```
+   
+   2. To define a stream transformer, use the following code:
+   
+      ```dart
+      var transformer = new
+          StreamTransformer.fromHandlers(handleData: convert);
+      ```
+   
+   3. To filter and transform the stream, and listen to its output to process further, use the following code:
+   
+      ```dart
+      stream
+        .where((value) => value[1] != 'U')
+        .transform(transformer)
+        .listen((value) => print("$value"));
+      }
+      ```

```
```dart
convert(value, sink) {
  // Create a new value from the original value
  var greeting = "Hello Mr. or Mrs. ${value[0]}";
  if (value[1] == 'F') {
    greeting = "Hello Mrs. ${value[0]}";
  }
}
```
```