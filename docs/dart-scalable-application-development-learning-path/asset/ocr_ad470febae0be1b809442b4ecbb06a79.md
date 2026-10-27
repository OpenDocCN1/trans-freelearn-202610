The result is as expected:

```
red
green
blue
```

Invocation of the moveNext method returns false after the collection ends, and the current pointer always returns the last element.

The for loop statement uses the Iterator transparently to iterate through the collection:

```
void main() {
  List<String> colors = ['red', 'green', 'blue'];

  for (String color in colors) {
    print(color);
  }
}
```

The result is similar to that of the preceding example.

# BidirectionalIterator

Sometimes, we need to iterate over a collection of elements in both directions. To help in such cases, Dart provides BidirectionalIterator. In the following code, BiListIterator is the implementation of BidirectionalIterator:

```
class BiListIterator<E> implements BidirectionalIterator<E> {
  final Iterable<E> _iterable;
  final int _length;
  int _index;
  E _current;
```

The constructor has an extra optional back parameter that defines the direction of the iteration:

```
BiListIterator(Iterable<E> iterable, {bool back:false}) :
  _iterable = iterable, _length = iterable.length,
  _index = back ? iterable.length - 1 : 0;

E get current => _current;
```