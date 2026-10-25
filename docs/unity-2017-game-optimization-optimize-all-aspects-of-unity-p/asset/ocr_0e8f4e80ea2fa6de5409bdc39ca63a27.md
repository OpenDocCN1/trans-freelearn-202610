If we're particularly savvy with C#, Visual Studio, and manual compilation of the Mono assembly, then we can have Visual Studio perform code compilation for us and copy the resulting assembly DLL into the `Assets` folder, which will fix this mistake for the generic collections.

Note that performing `foreach` over a `Transform` Component is a typical shortcut to iterating over a `Transform` Component's children. Let's consider the following example:

```
```
foreach (Transform child in transform) {
    // do stuff with 'child'
}
```
```

However, this results in the same heap allocations mentioned above. As a result, that coding style should be avoided in favor of the following code:

```
```
for (int i = 0; i < transform.childCount; ++i) {
    Transform child = transform.GetChild(i);
    // do stuff with 'child'
}
```
```

# ## Coroutines

As mentioned before, starting a Coroutine costs a small amount of memory to begin with, but note that no further costs are incurred when the method calls `yield`. If memory consumption and garbage collection are significant concerns, we should try to avoid having too many short-lived Coroutines and avoid calling `StartCoroutine()` too much during runtime.

# ## Closures

Closures are useful, but dangerous tools. Anonymous methods and lambda expressions are not always Closures, but they can be. It all depends on whether the method uses data outside of its own scope and parameter list or not.

For example, the following anonymous function would not be a Closure, since it is self-contained and functionally equivalent to any other locally defined function:

```
```
System.Func<int,int> anon = (x) => { return x; };

int result = anon(5); // result = 5
```
```