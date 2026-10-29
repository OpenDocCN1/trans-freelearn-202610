```
#{}
;;=> #{}
(hash-set)
```

We can use `first`, `rest`, and `conj` for sets. Sets do not keep the order of elements as you input data, so `first` or `rest` may look different to what you expected:

```
(first #{:a :b :c :d :e})
;;=> :e
(rest #{:a :b :c :d :e})
;;=> (:c :b :d :a)
(conj #{:a :b :c :d} :e)
;;=> #{:e :c :b :d :a}
(conj #{:a :b :c :d :e} :e)
;;=> #{:e :c :b :d :a}
```

The `contains?` function examines whether the element exists in a set, whereas `disj` eliminates the element from the set:

```
(contains? #{:a :b :c :d :e} :c)
;;=> true
(contains? #{:a :b :c :d :e} :f)
;;=> false
(disj #{:a :b :c :d :e} :c)
;;=> #{:e :b :d :a}
(disj #{:a :b :c :d :e} :c :d)
;;=> #{:e :b :a}
```

# How it works...

All collection types in Clojure are persistent and immutable. A persistent data structure preserves the previous versions of itself when it is modified. Clojure has efficient implementation to create modified versions of collections so it doesn't simply copy the entire data structure, but shares the original data.

# Clojure collections

All collection types implement the `clojure.lang.Seqable` interface and its `seq` method returns `clojure.lang.ISeq`. `ISeq` has the `cons`, `first`, `more`, and `next` functions: