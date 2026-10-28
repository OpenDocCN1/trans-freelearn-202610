JSON objects can also contain arrays, using square brackets:

```
{
  "key1": "value1",
  "array":
  [
    "First item in array",
    "Second item in array",
    "Third item in array"
  ]
}
```

They can even contain other JSON objects, by nesting curly braces:

```
{
  "key1": "value1",
  "subObject":
  {
    "subKey1": "subValue1",
    "subKey2": "subValue2",
  }
}
```

These sub-objects can contain other objects and arrays, and arrays can contain other objects or arrays, too.

Note that this is very similar to the AS3 syntax for declaring an object:

```
var as3Object:Object = {
  key1:"value1",
  key2:"value2",
  subObject:{
    subKey1:"subValue1"
  },
  myArray:[1, 2, 3]
}
```

For more information, check out http://www.json.org.

Unlike with XML, AS3 has no native features for handling JSON objects – but there is an officially supported library that does.