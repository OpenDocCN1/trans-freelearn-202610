# # Map collectors

The streams can also be collected as a map; however, the key-value pairs need to be identified in order to create a map:

```
```java
//Map Collectors
Map<String , Integer>mapCollected=orderedSetCollected.stream().collect(Collectors.t
System.out.println("The generated Map values are : : "+mapCollected);
```
```

In the preceding implementation, it is assumed that the keys are unique; however, that may not be always the case and we might get an
IllegalStateException exception saying that a duplicate key exists. To handle such scenarios, an overloaded method of toMap() can be used as follows:

```
```java
//Map Collectors with duplicate key handling
Map<Object, List<Integer>> mapWithDupVals=streamSupplier.get().collect(Collectors.t
//KeyMapper
x - > {List <Integer>tmp = new ArrayList <> (); tmp.add(x.toString().length()); retu
//ValueMapper
(L1, L2) - > { L1.addAll(L2); returnL1;} //MergeFunction
));
System.out.println("The generated Map values with duplicate values::" + mapWithDupV
```
```

Here the toMap() method accepts three arguments: KeyMapper, ValueMapper, and
MergeFunction. The role of KeyMapper is to produce the key value of the map,
while the role of ValueMapper is to map the value in this case in a list. Merge
function has a special role of conflict avoidance as per the logic of the
function, here the logic being to add both the elements in a list. There can be
multiple ways to handle duplicate keys; the preceding case is only one of the
many ways of doing so.