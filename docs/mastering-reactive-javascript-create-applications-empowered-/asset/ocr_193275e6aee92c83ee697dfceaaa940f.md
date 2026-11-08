To run a transducer over an array we first need a method to run a given transformation in a given iterable object.

For this we use the into() method, which has the following signature:

```
transducers.into(emptyIterable,transformation,iterable)
```

It receives three parameters and they are all mandatory:

+   - emptyIterable: It is an empty iterable used to store the result of the application of the transformation over the iterable object
+   - transformation: It is the function or transducers to be applied over the iterable object
+   - iterable: It is the iterable object to be transformed

With the usage of this function we can implement transformations over an array, let's see how we can implement a simple map with it:

```
var t = require('transducers-js');

var iterableObject = [1,2,3,4];
var mapTransducer = t.map((i)=>i+1);

var result = t.into(
  [],
  mapTransducer,
  iterableObject
);

console.log(result);
```

This example does the following:

+   1. Imports the transducers-js library and stores it in the t variable.
+   2. Creates an array called iterableObject, where the transformation will be applied; the array contains the value [1,2,3,4].
+   3. Creates a transducer to map the values of an iterable object in it plus one, called mapTransducer.
+   4. Calls the into method to iterate the iterableObject using the mapTransducer into an empty array passed as the first argument of the method; the result of this is stored in the result variable.