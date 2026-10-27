# Raw types

Dart supports arrays in the form of the List class. Let's say you use a list to store data. The data that you put in the list depends on the context of your code. The list may contain different types of data at the same time, as shown in the following code:

```
// List of data
List raw = [1, "Letter", {'test':'wrong'}];
// Ordinary item
double item = 1.23;

void main() {
  // Add the item to array
  raw.add(item);
  print(raw);
}
```

In the preceding code, we assigned data of different types to the raw list. When the code executes, we get the following result:

```
[1, Letter, {test: wrong}, 1.23]
```

So what's the problem with this code? There is no problem. In our code, we intentionally used the default raw list class in order to store items of different types. But such situations are very rare. Usually, we keep data of a specific type in a list. How can we prevent inserting the wrong data type into the list? One way is to check the data type each time we read or write data to the list, as shown in the following code:

```
// Array of String data
List parts = ['wheel', 'bumper', 'engine'];
// Ordinary item
double item = 1.23;

void main() {
  if (item is String) {
    // Add the item to array
    parts.add(item);
  }
  print(parts);
}
```