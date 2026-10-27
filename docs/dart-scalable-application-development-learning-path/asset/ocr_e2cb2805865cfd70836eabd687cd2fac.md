```
@override
int get length => _elements.length;

@override
void set length(int newLength) {
  _elements.length = newLength;
}
}
```

You might be surprised to know that you need to implement only four methods to have a fully functional list-based class. This is because the other methods of the ListBase class use those four main methods to manage the internal buffer and iterate over elements. If you do not desire to extend the ListBase class, you can use ListMixin as follows:

```
class OtherList<E> extends MainList with ListMixin<E> {
  // ...
}
```

The List class interface supports ordering via a sort method.

> The asMap method of the List class returns a Map view that cannot be modified.

Sometimes, we need to randomly rearrange the elements in the List class. The shuffle method of the List class can come in handy while doing that:

```
import 'dart:math';

main() {
  var list = new List.from([1, 2, 3, 4, 5]);
  print(list);
  // => [1, 2, 3, 4, 5]
  // Crete seed to initialize internal state of
  // random-number generator
  var seed = new DateTime.now().millisecondsSinceEpoch;
  // Create instance of generator
  var random = new Random(seed);
  // Re-arrange elements in list
  list.shuffle(random);
  print(list);
  // => [4, 5, 1, 3, 2]
}
```