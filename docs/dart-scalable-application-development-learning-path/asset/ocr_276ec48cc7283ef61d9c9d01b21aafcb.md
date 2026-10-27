2. The `_onKeyPress` method is as follows:

```
```dart
_onKeyPress(KeyboardEvent e) {
  if (e.keyCode == KeyCode.ENTER) submit(e);
}
```
```

Now, pressing *Enter* will cause the form to be submitted.

# How it works...

The document object from `dart:html` has three events to work with key input: `onKeyPress`, `onKeyDown`, and `onKeyUp`. They all generate a stream of `KeyboardEvent` objects that capture user interaction with the keyboard. However, these events are also defined for `window`, the parent object of `document`, and any `Element` on the page, so you can use them very specifically on a certain `InputElement` or `<div>` region.

The `keyCode` of the event is an integer from a list of constants, which specifies a value for every key, such as `KeyCode.A`, `KeyCode.SPACE`, and `KeyCode.LEFT` for the left arrow key, and so on. This lends itself very easily to a `switch/case` construct, as follows:

```
```dart
_onKeyPress(KeyboardEvent e) {
  e.preventDefault();
  print('The charCode is ${e.charCode}');
  if (e.keyCode == KeyCode.ENTER) submit(e);
  if (e.ctrlKey) return;

  switch(e.keyCode) {
    case KeyCode.DOWN:
      // move sprite down
      break;
    case KeyCode.UP:
      // move sprite up
      break;
    case KeyCode.F1:
      // show help
      break;
  }
}
```
```

The event also has an integer character code getter named `charCode`. If the key was a special key, this can also be tested with `e.ctrlKey`, `e.altKey`, `e.metaKey`, `e.ShiftKey`, and `e.altGraphKey`:

```
```dart
if (e.ctrlKey) return;
```
```