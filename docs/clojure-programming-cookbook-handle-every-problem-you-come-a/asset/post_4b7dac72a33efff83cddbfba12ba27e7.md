
get looks up a map entry by its key and returns its value. If get cannot find the key in a collection, it returns nil.

If you don't want to obtain nil in such a case, specify the value you want when the key does not exist:

```
(get {:a 1, :b 2, :c 3 :d 4 :e 5} :c)
;;=> 3
(get {:a 1, :b 2, :c 3 :d 4 :e 5} :f)
;;=> nil
(get {:a 1, :b 2, :c 3 :d 4 :e 5} :f :not-found)
;;=> :not-found
```

assoc adds key-value pairs to maps. If there is already a key-value pair in the map, it replaces the value:

```
(assoc {:a 1, :b 2, :c 3 :d 4} :e 5)
;;=> {:a 1, :b 2, :c 3, :d 4, :e 5}
(assoc {:a 1, :b 2, :c 3 :d 4} :c 5)
;;=> {:a 1, :b 2, :c 5, :d 4}
```

assoc can use multiple key-value pairs as its arguments:

```
(assoc {:a 1, :b 2, :c 3 :d 4} :e 5 :f 6)
;;=> {:a 1, :b 2, :c 3, :d 4, :e 5, :f 6}
```

The dissoc removes key-value pairs from a map:

```
(dissoc {:a 1, :b 2, :c 3 :d 4} :c)
;;=> {:a 1, :b 2, :d 4}
(dissoc {:a 1, :b 2, :c 3 :d 4} :c)
;;=> {:a 1, :b 2, :d 4}
(dissoc {:a 1, :b 2, :c 3 :d 4} :e)
;;=> {:a 1, :b 2, :c 3, :d 4}
```

## Sets

Sets maintain the uniqueness of their elements. To create a set, beginning with a hash (#) and then enclose elements with {}. If you want to create an empty set, use #{} or (hash-set):

```
#{"apple" "orange" "banana" "peach" "strawberry"}
;;=> #{"peach" "apple" "banana" "orange" "strawberry"}
```

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

## How it works...

All collection types in Clojure are persistent and immutable. A persistent data structure preserves the previous versions of itself when it is modified. Clojure has efficient implementation to create modified versions of collections so it doesn't simply copy the entire data structure, but shares the original data.

## Clojure collections

All collection types implement the `clojure.lang.Seqable` interface and its `seq` method returns `clojure.lang.ISeq`. `ISeq` has the `cons`, `first`, `more`, and `next` functions:

```
public interface Seqable {
    ISeq seq();
}
```

```
public interface ISeq extends IPersistentCollection {
    Object first();
    ISeq next();
    ISeq more();
    ISeq cons(Object o);
}
```

ISeq implements IPersistentCollection and it has count, cons, empty, and equiv:

```
public interface IPersistentCollection
    extends Seqable {
    int count();
    IPersistentCollection cons(Object o);
    IPersistentCollection empty();
    boolean equiv(Object o);
}
```

## Differences between lists and vectors

Lists and sets represent sequences of elements, but there are some differences:

- Lists are `clojure.lang.PersistentList` and vectors are `clojure.lang.PersistentVector`.
- Lists are implemented as a linked list. Vectors are implemented using a tree structure, and each node has 32 elements.
- Vectors are efficient for index access. Index access for lists takes O(n), whereas it takes O(log32(n)) for vectors.
- That is why, with `conj`, the order of the results is different in lists and vectors. `conj` puts an element at the beginning of lists, but it puts an element at the end of vectors:

```
(conj '(2 3 4 5) 1)
;;=> (1 2 3 4 5)
(conj [1 2 3 4] 5)
;;=> [1 2 3 4 5]
```

The following code tests index access for a list and a vector. This test measures access to the last element in a collection consisting of 1 million

elements. The test loops 100 times. Access to the list takes half a second, but access to the vector only takes 0.1 milliseconds:

```
(let [n 1000000 iter 100
      l (into () (repeat n "a"))
      v (into [] (repeat n "a"))
      ]
  (time (dotimes [_ iter] (nth l (dec n) )))
  (time (dotimes [_ iter] (nth v (dec n) )))
)
"Elapsed time: 532.623841 msecs"
"Elapsed time: 0.167739 msecs"
```

Reversing the order of vectors is faster than reversing the order of lists:

```
(do
  (time (reverse (into '() (range 100000))))
  (time (reverse (into [] (range 100000))))
  nil
)
"Elapsed time: 60.67435 msecs"
"Elapsed time: 26.431954 msecs"
nil
```

The reason is that clojure.lang.PersistentVector implements clojure.lang.Reversible but clojure.lang.PersistentList does not.

## Clojure is immutable

Since Clojure functions for collections are immutable, it does not modify the original collection. It is different from Python or Ruby. In the following Python code, insert modifies the original list:

```
l = [2, 3, 4, 5]
l.insert(0, 1)
l
#[1, 2, 3, 4, 5]
```

The next Ruby code also modifies the original list:

```
$ irb
irb(main):001:0> letters = Array.new([2,3,4])
=> [2, 3, 4]
irb(main):002:0> letters.insert(0, 1)
```

```
=> [1, 2, 3, 4]
```

But Clojure does not change the original data:

```
(def l '(2 3 4 5))
#'collection.core/l
(conj '(2 3 4 5) 1)
;;=> (1 2 3 4 5)
l
;;=> (2 3 4 5)
```

Using immutable data ensures that arguments are unmodified after returning from other function calls. On the other hand, calling functions causes unexpected modification of arguments, which causes bugs that are difficult to find.

## There's more...

Here, we will show you some techniques for manipulating collections.

## Converting collections between different types

into converts types in collections. The following code converts between a vector and a list:

```
(into [] '(1 2 3 4 5))
;;=> [1 2 3 4 5]
(into '() [1 2 3 4 5])
;;=> (5 4 3 2 1)
```

In the following code, into converts between a map and a vector:

```
(into {} [[:a 1][:b 2] [:c 3][:d 4][:e 5]])
;;=> {:a 1, :b 2, :c 3, :d 4, :e 5}
(into [] {:a 1 :b 2 :c 3 :d 4 :e 5})
;;=> [[:a 1] [:b 2] [:c 3] [:d 4] [:e 5]]
```

Finally, this into converts between a set and a vector:

```
(into [] #{:a :b :c :d :e})
;;=> [:e :c :b :d :a]
(into #{} [:a :b :c :d :e])
;;=> #{:e :c :b :d :a}
```

## Using distinct

distinct is similar to `into` with sets. It eliminates duplicated elements:

```
(distinct ["apple" "orange" "melon" "grape" "orange" "pinapple"])
;;=> ("apple" "orange" "melon" "grape" "pinapple")
(into #{} ["apple" "orange" "melon" "grape" "orange" "pinapple"])
;;=> #{"apple" "pinapple" "orange" "grape" "melon"}
```

Unlike `set`, using `distinct` does not change the data type.

## Accessing and updating elements from collections

In this recipe, we will teach you how to access elements and update elements in collections.

## Getting ready

You only need REPL, as described in the recipe in Chapter 1, Live Programming with Clojure, and no additional libraries. Start REPL so that you can review the sample code in this recipe.

## How to do it...

Let's start with accessing collections.

## Accessing collections using the nth function

nth gets the nth element from collections. The second argument of nth starts from 0 and throws an exception if the second argument is larger than the number of elements minus 1:

```
(nth [1 2 3 4 5] 1)
;;=> 2
(nth '("a" "b" "c" "d" "e") 3)
;;=> "d"
(nth [1 2 3] 3)
;;=> IndexOutOfBoundsException
clojure.lang.PersistentVector.arrayFor
(PersistentVector.java:153)
```

If you would like to avoid such an exception, use the third argument as the return value:

```
(nth [1 2 3] 3 nil)
;;=> nil
```

Notice that nth does not work with maps and sets.

## Accessing maps or sets using get

get accesses maps and sets using a key, and if there is a corresponding key in a map or set, it returns its value. If there is not the same key in a map or set, it returns nil or the third argument:

```
(get {:a 1 :b 2 :c 3 :d 4 :e 5} :c)
;;=> 3
(get {:a 1 :b 2 :c 3 :d 4 :e 5} :f)
;;=> nil
(get {:a 1 :b 2 :c 3 :d 4 :e 5} :f :not-found)
;;=> :not-found

(get #{:a :b :c} :c)
;;=> :c
(get #{:a :b :c} :d)
;;=> nil
(get #{:a :b :c} :d :not-found)
;;=> :not-found
```

## Maps, sets, and keywords are functions to access collections

It is an idiomatic Clojure approach to use maps or sets as the first argument. The following code is the same as using get:

```
;;=> :not-found
```

Using keywords is also Clojure-idiomatic. This is the same as get:

```
(:c {:a 1 :b 2 :c 3 :d 4 :e 5})
;;=> 3
(:f {:a 1 :b 2 :c 3 :d 4 :e 5})
;;=> nil
(:f {:a 1 :b 2 :c 3 :d 4 :e 5} :not-found)
;;=> :not-found
```

get for maps returns the key if the key exists in the elements. It returns nil if there is no matching key:

```
(get #{:banana :apple :strawberry :orange :melon} :orange)
;;=> :orange
```
