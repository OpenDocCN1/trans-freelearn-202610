With HashSet, we can control the consistent equality via the constructor arguments:

```
import 'dart:collection';

void main() {
  var hset = new HashSet(equals: (e1, e2) {
    return e1 == e2;
  }, hashCode: (e) {
    return e.hashCode;
  });
  hset.addAll([3, 2, 3, 1]);
  print(hset);
  // => {1, 2, 3}
  hset.add(1);
  print(hset);
  //  => {1, 2, 3}
}
```

The constructor's named argument, equals, must be a function to compare the equality of two elements in the collection. Another constructor's named argument, hashCode, must be a function that calculates the hash code of the specified element. If both the elements are deemed equal, then they should return the same hash code. If both named arguments are omitted, the Set class uses the internal equals and hashCode methods of the element.

# LinkedHashSet

The LinkedHasSet class is an ordered hash-table-based Set implementation that maintains the insertion order of elements for iteration and runs nearly as fast as HashSet. The order of adding items to a collection determines the order of iteration via elements of the collection. The consistent equality in LinkedHasList is defined by the equals operator and mostly based on the value of the hashCode method. Adding an element that is already in Set does not change its position in the iteration order, but removing an element and adding it again will make it the last element of iteration:

```
import 'dart:collection';

void main() {
  var hset = new LinkedHashSet();
  hset.addAll([3, 2, 3, 1]);
  print(hset);
  // => {3, 2, 1}
  hset.add(1);
  print(hset);
  //  => {3, 2, 1}
}
```