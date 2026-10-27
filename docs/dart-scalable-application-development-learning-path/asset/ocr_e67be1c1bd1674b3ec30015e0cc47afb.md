Any chance that my collection will be initialized in this way in the future is high. So, it would be nice to have an initialization method in my `Collection` class to add a list of data. One of the solutions is to create a constructor with named parameters to manage our initialization. The code creates a new collection from the optional parameter value if specified, as shown in the following code:

```
```dart
library collection;

// Collection class
class Collection {
  // We save data here
  List _data;

  // Default constructor with optional [values] or [item].
  Collection({Iterable values:null, String item:null}) {
    if (item != null) {
      _data = new List();
      add(item);
    } else {
      _data = values != null ?
          new List.from(values) :
          new List();
    }
  }

  // Add new item
  add(item) {
    _data.add(item);
  }

  // ...
}
```
```

This solution has a *right to live*, but only if the number of parameters is small. Otherwise, a simple task initialization of variables results in very complicated code. I have specified two options for the named parameters with a really tangled logic. It is difficult to convey the meaning of what `values` means. A better way is to use named constructors, as shown in the following code:

```
```dart
library collection;

// Collection class
class Collection {
  // We save data here
  List _data;
```
```