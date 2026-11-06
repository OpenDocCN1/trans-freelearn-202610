The index would now look like this:

```
2,"pens"
5,"pens"
10,"books"
12,"pens"
13,"books"
15,"books"
15,"pens"
22,"books"
```

In this index, when we navigate to the index range where price is less than 20, we’ll find that category is all mixed. This means we’ll have to scan more index entries with a price less than 20, throwing away ones that don’t match the equality predicate, than we would with a different index structure. And what if we needed a sort on the second field after applying range on the first field? Again, because the values are not in order, we would need to do a lot more work in memory.

The same concept applies if we have equality and sorting. If the equality is on the first field in the compound index, then the second field values are still “in order” in the part of the index we have navigated to. But if sort is on the first field in the index, then while we can get those values in order while scanning the full index, we have to keep checking and throwing away the entries where the second field is not equal to our predicate.

What if we have all three: equality, range predicate, and a sort on another field? Let’s return to our example with three fields in the {category:1,price:1,stock:1} index and these values:

```
"books",10,399
"books",13,205
"books",15,11
"books",22,0
"pens",2,175
"pens",5,42
"pens",12,109
"pens",15,98
```