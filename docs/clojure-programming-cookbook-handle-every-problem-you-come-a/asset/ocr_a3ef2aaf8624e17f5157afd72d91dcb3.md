function in our namespace. We need to give the full path to fn now to define the definition, so clojure.core/fn:

```
hello=> (def b (clojure.core/fn[c] (clojure.core/inc c)))
#'hello/b
hello=> (b 2)
3
```

# Inspecting namespaces

Namespaces are regular objects, so on the JVM we can see their internals very easily. find-ns tells us what the object is, and on the JVM we then call .getMappings from the namespace object:

```
(find-ns 'hello)
; #object[clojure.lang.Namespace 0x3b5fad2d "hello"]
(.getMappings *1)
....
```

Namespaces are listed and retained statically in a static field of this class on the JVM so we can refer to things later.

While in-ns helps us define namespaces, we will actually use a higher-level function named ns:

```
helloagain=> (ns helloagain)
nil
helloagain=>
```

If you want to go and have a look, ns actually does quite a bit for us. The two main keywords that can be used are imports and refers.

# Adding functions with :import and :refer

:import will import classes from the hosting virtual machine (JVM, JavascriptVM, and .NET VM for now). Say in Java we want to use the raw Random Java object directly, we will import it in the namespace with the following:

```
(ns helloagain (:import [java.util Random]))
(Random.)
; #object[java.util.Random 0x6337c201
```