```
@deprecated
  Embrace.strangle(): _strength = 100;

@override consumedCalories() { } //      warning!
}

class Movement {
  String Name;
  Movement();
  consumedCalories() {
    // calculation of calories
  }
}

class ToFix {
  final String note, author, date;
  const ToFix(this.note, this.author, {this.date});
}
```

# How it works...

The @deprecated instance is used to indicate something that you no longer want users of your library to use, and that will probably stop working in a future version. When the analyzer in Dart Editor sees this annotation, it marks the code component that follows (and everywhere it is used) with a strike-through line. Moreover, it will give a warning: '...' is deprecated. The @override instance is also a good (but not necessary) indication that you want to override an inherited behavior. Here, the editor also uses this instance to point to possible bugs; if you had written @override consumedcalories(), then you would get the warning Method does not override an inherited method; so typos are eliminated.

To make your own annotations, you must make sure that it is defined as a constant expression that starts with an identifier, such as const Anno in the example, which could be used as @Anno. More specifically, it must be a reference to a compile-time constant variable or call to a constant constructor. We used the class ToFix to give our class the annotation @ToFix("Improve the algorithms").

# There's more...

Annotations are defined in Dart through the class Annotation in the analyzer library. With the reflection mirror library (see the Using reflection recipe), it is possible to extract metadata at runtime and use its values to influence program execution.