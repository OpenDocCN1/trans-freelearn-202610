```
public Object invoke()
{
    return invokeStatic();
}

public static final Var const__0 =
(Var)RT.var("clojure.core", "println");

public static Object invokeStatic()
{
    return ((IFn)const__0.getRawRoot()).invoke("hello world");
}
}
```

The main Java objects being used in Clojure are as follows:

+   - `clojure.lang.IFn`: This is Clojure function, something that can be invoked.
+   - `clojure.lang.A`: This function is mostly an annotated Clojure function.
+   - `clojure.lang.RT`: This is the Clojure runtime.
+   - `clojure.lang.Var`: This is a variable, a threaded reference, that is also a IFn and so can be invoked.

With this in mind, we can see that the Clojure `println` function from `clojure.core` is being kept as a `Var` from within the `gen3` class:

```
public static final Var const__0 = (Var)RT.var("clojure.core", "println");
```

The `Var` is retrieved through questioning the runtime for a var through `RT.var`, a namespace, and a function.

What happens when the `-main` method is called is simply a static call to `invokeStatic`. This is rather generic, and calls invoke on an `IFn` object:

```
((IFn)const__0.getRawRoot()).invoke("hello world")
```

`const__0` is here a generated reference to the `println` function in the namespace `clojure.core`. Calling `-main` results in executing the body of `invokeStatic`, which, in turn, invokes the `println` function with parameters "hello world".