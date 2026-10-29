Here we have:

```
(list println "FIRST")
```

When typed in an REPL, that list will expand to the following:

```
; (#object[clojure.core$println 0xe1e81f5
"clojure.core$println@e1e81f5"] "FIRST")
```

So basically, you have a list made of two elements: a function and a string. This list is ready to be compiled and can then be evaluated like the usual Clojure code.

To evaluate the list, we can use the eval function that will evaluate the Clojure code:

```
(eval (list println "FIRST"))
; nil, but "FIRST" is printed in the console ..
```

Right, so we now understand that a macro is generating Clojure code, which is a list:

```
(my-very-first-macro)
; nil, but "FIRST" is printed in the console ..
```

We think we are writing (my-very-first-macro), but the code generated is actually the following:

```
(println "FIRST")
; yes, this is a list!
```

# Your second macro

The second macro will build on the first one by simply making use of parameters in the macro definition.

Our second macro will be a macro that multiplies its input by three. To do so, we want to write something with defmacro, which returns a list made of *, 3, and the input.

Try it: