Dart reflection follows the second approach with Mirrors. You can find more information about the concept of Mirrors in the original paper written by Gilad Bracha at http://bracha.org/mirrors.pdf. Let's discuss the advantages of Mirrors:

+   - Mirrors are separate from the main code and cannot be exploited for malicious purposes
+   - As reflection is not part of the code, the resulting code is smaller
+   - There are no method-naming conflicts between the reflection API and inspected classes
+   - It is possible to implement many different Mirrors with different levels of reflection privileges
+   - It is possible to use Mirrors in command-line and web applications

Let's try Mirrors and see what we can do with them. We will continue to create a library to run our tests.

# Introspection in action

We will demonstrate the use of Mirrors with something simple such as introspection. We will need a universal code that can retrieve the information about any object or class in our program to discover its structure and possibly manipulate it with properties and call methods. For this, we've prepared the TypeInspector class. Let's take a look at the code. We've imported the dart:mirrors library here to add the introspection ability to our code:

```
library inspector;

import 'dart:mirrors';
import 'test.dart';

class TypeInspector {
  ClassMirror _classMirror;
  // Create type inspector for [type].
  TypeInspector(Type type) {
    _classMirror = reflectClass(type);
  }
}
```