Using expand does not work if the list contains ints (or Strings, doubles, and so on) as single list elements, or if there are multiple levels of nesting. In that case, we will have to work recursively, as implemented in the flatten method:

```
Iterable flatten(Iterable iterable)
  => iterable.expand((e) => e is List ? flatten(e) : [e]);
```

# There's more...

Why would you want to flatten a list of lists? There may be application needs to do this, for example, when you use two-dimensional lists or matrices in the game logic, but an obvious reason is that working with a list of lists is much more expensive performance wise.

# Generating a random number within a range

You may have often wondered how to generate a random number from within a certain range. This is exactly what we will look at in this recipe; we will obtain a random number that resides in an interval between a minimum (min) and maximum (max) value.

# How to do it...

This is simple; look at how it is done in random_range.dart:

```
import 'dart:math';

var now = new DateTime.now();
Random rnd = new Random();
Random rnd2 = new Random(now.millisecondsSinceEpoch);

void main() {
  int min = 13, max = 42;
  int r = min + rnd.nextInt(max - min);
  print("$r is in the range of $min and $max"); // e.g. 31
  // used as a function nextInter:
  print("${nextInter(min, max)}"); // for example: 17

  int r2 = min + rnd2.nextInt(max - min);
  print("$r2 is in the range of $min and $max"); // e.g. 33
}
```