Suppose you have an undocumented method that returns a Future value and you want to know the properties and methods of that object without digging into the source code. Run the following code snippet:

```
import 'dart:mirrors';

undocumentedMethod().then((unknown){
    var r = reflect(unknown).type;  // ClassMirror
    var m = r.declarations;
    for (var k in m.declarations.keys) print(MirrorSystem.getName(k));
});
```

# See also

+   - When you want to use reflection in the code, which has to be minified and tree shaken, read the *Shrinking the size of your app* recipe in *Chapter 1, Working with Dart Tools*

# Using mixins

In Dart, just like in Ruby, your classes can use mixins to assign a certain behavior to your class. Say an object must be able to store itself, so its class mixes in a class called `Persistable` that defines `save()` and `load()` methods. From then on, the original class can freely use the mixed-in methods. The mechanism is not used for specialized subclassing or is-a relationships, so it doesn't use inheritance. This is good because Dart uses a single inheritance, so you want to choose your unique direct superclass with care.

# How to do it...

+   - Look at the `mixins` project; the Embrace class from the previous recipe needs to persist itself, so it mixes with the abstract class `Persistable`, thereby injecting the save and load behavior. Then, we can apply the `save()` method to the `embr` object, thereby executing the code of the mixin as follows:

```
void main() {
    var embr = new Embrace(5);
```

+   - Using the mixins methods, as shown in the following code:

```
print(embr.save(embr.strength));
print(embr is Movement);  // true
print(embr is Persistable); // true
}
```