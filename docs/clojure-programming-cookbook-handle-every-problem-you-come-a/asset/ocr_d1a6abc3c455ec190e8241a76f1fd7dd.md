```
if(RT.booleanCast(o))
    return -1;
  return RT.booleanCast(invoke(o2,o1))? 1 : 0;
}
Number n = (Number) o;
return n.intValue();
}
```

So, if the function returns a Boolean and that Boolean is true, we suppose the first object is smaller. Then we compare the Boolean value of the reverse invoke call to find out if the values are equal or not.

Finally, if we're not in the Boolean domain, we use numeric values to compare:

```
(sort
  (fn[_ _] -1)
  [1 2 3 0 -1])
; (-1 0 3 2 1)
```

Clojure sort calls on the JVM eventually rely on the JVM's own sort implementation, which is a modified version of the merge sort algorithm.

There is more to explain about sorting on the JVM, but we'll leave it for now as it is out of the scope of this recipe.

In any case, sort in Clojure should be as fast as sort on Java collections.

# There's more...

Sorting is a fun sport on its own and we could go through all sorts of algorithms. Here, we will focus on how to use sorting in day-to-day tasks.

# Sorting lines of a file

In this small example, we will learn how to sort lines of text read from a file. The following example will load the whole file in memory, so it may not be appropriate for most big files, but we can see how to directly apply what we've learned about sorting to organize lines in a file: