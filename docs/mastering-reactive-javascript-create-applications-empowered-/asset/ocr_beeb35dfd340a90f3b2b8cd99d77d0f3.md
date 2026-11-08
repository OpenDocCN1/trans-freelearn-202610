The code can be described by the following diagram:

![](bbox=[0.04, 0.14, 0.93, 0.5])

Notice that the transducer passed as an argument to the transduce operator is the same transducer used to apply transformations in an array in the previous section; we can even use the same transducer in two different objects, as can be seen in the following code:

```
var mapTransducer = t.map((i)=>i+1);

Rx.Observable
.of(1,2,3,4)
.transduce(mapTransducer)
.subscribe((i)=>console.log(i));

var resultArray = t.into(
  [],
  mapTransducer,
  [1,2,3,4]
);
console.log(resultArray);
```