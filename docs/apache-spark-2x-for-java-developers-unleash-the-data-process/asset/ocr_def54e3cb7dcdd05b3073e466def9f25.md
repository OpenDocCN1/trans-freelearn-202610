# generics:

```
| List<Integer> listGeneric = new ArrayList<>();
listGeneric.add(1);
listGeneric.add(2);
listGeneric.add("hello"); // won't compile
```

listGeneric becomes the list of integers. If you try to add strings in it, the compiler will not allow it.

So, now if you retrieve an element from the list, it will give you an integer type. No type casting is required, which will save us from ClassCastException at runtime. Hence, it provides compile-time safety:

```
| Integer intObject = listGeneric.get(0);
```