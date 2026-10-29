# How to become lazy

As you may know, laziness is a simple concept. It is the act of only doing the minimum required, or to postpone work until the very last moment. This is a conscious choice, and therefore must be applied with care.

One of the reasons Clojure is enjoyable as a programming language is because it makes it easy to be lazy; or, in simple computing terms, it only evaluates the minimum needed, when it is needed. Nothing extra.

So, why would you want to be lazy, or in other words, build lazy sequences? Obviously, to avoid unnecessary computations. An example of this is, say, if you have a mathematical formula or a repeating function defining a list of elements and you only want to use some of those elements. This is when you want to write a lazy sequence, and not evaluate all the elements of the list up front but only those that will be accessed and used at runtime.

In this recipe, we will see first how to use lazy sequences in Clojure, as well as create our own.

# Getting ready

For this section, we do not need anything else but a running REPL. To prepare for laziness, though, we can have look at the source of the lazy-seq macro, which is the way we can create lazy collections:

```
(defmacro lazy-seq
  [& body]
  (list 'new 'clojure.lang.LazySeq (list* '^{:once true} fn* []
body)))
```

Without going into too much detail, we can see the Clojure code delegates calls to a Java object, LazySeq, https://github.com/clojure/clojure/blob/master/src/jvm/clojure/lang/LazySeq defined from a single function.