
(get #{:banana :apple :strawberry :orange :melon} :grape)
;;=> nil
(get #{:banana :apple :strawberry :orange :melon} :grape :not-found)
;;=> :not-found

We can use a set or a keyword as the first argument, but there is a third argument in this set:

```
(#{:banana :apple :strawberry :orange :melon} :orange)
;;=> :orange
(#{:banana :apple :strawberry :orange :melon} :grape)
;;=> nil
(:orange #{:banana :apple :strawberry :orange :melon})
;;=> :orange
(:grape #{:banana :apple :strawberry :orange :melon})
;;=> nil
```

## Accessing a collection using second, next, ffirst, and nfirst

second returns the second element from a collection:

```
(second [1 2 3 4 5])
;;=> 2
(second '())
;;=> nil
```

The behavior of next is almost the same as rest, but next returns nil when the result is empty:

```
(next [1])
;;=> nil
(rest [1])
;;=> ()
```

ffirst returns the first element of the first element:

```
(ffirst [[1 2 3] 4 [3 5 6]])
;;=> 1
(ffirst {:a 1 :b 2})
;;=> :a
```

This is equivalent to the following code:

```
(first (first [[1 2 3] 4 [3 5 6]]))
```

```
;;=> 1
(first (first {:a 1 :b 2}))
;;=> :a
```

nfirst returns the next element of the first element:

```
(nfirst [[1 2] [3 4][5 6]])
;;=> (2)
(nfirst {:a 1 :b 2})
;;=> (1)
```

## Using update to update collections

update updates the matched value using the function specified by the third element:

```
(update {:a 1 :b 2 :c 3} :a inc)
;;=> {:a 2, :b 2, :c 3}
```

## How it works...

Maps, sets, and vectors implement clojure.lang.IFn. Clojure functions such as + implement IFn. This is the reason why maps and sets can be functions. ifn? tests whether an argument implements clojure.lang.IFn:

```
(ifn? +)
;;=> true
(ifn? [])
;;=> true
(ifn? {})
;;=> true
(ifn? #{})
;;=> true
(ifn? :a)
;;=> true
(ifn? '())
;;=> false
(ifn? 1)
;;=> false
```

Vectors and maps are functions, but lists and integers are not functions.

## There's more...

Here, we will teach you some functions that are useful for accessing and updating collections.

## Using get for vectors

Like with maps and sets, get works with vectors:

```
(get ["a" "b" "c" "d" "e"] 3)
;;=> "d"
(get ["a" "b" "c" "d" "e"] 5)
;;=> nil
(get ["a" "b" "c" "d" "e"] 5 :not-found)
;;=> :not-found
```

Vectors are also functions:

```
(["a" "b" "c" "d" "e"] 3)
;;=> "d"
(["a" "b" "c" "d" "e"] 5)
;;=> IndexOutOfBoundsException
clojure.lang.PersistentVector.arrayFor
(PersistentVector.java:153)
```

Using get for lists always returns nil. Never do that:

```
(get '(1 2 3) 1)
;;=> nil
```

## Using collections as keys in maps

Unlike Java, Python, and Ruby, Clojure can use collection types as keys. The next map type uses maps as keys and finds the value using a map key:

```
(def location
  {{:x 1 :y 1} "Nico" {:x 1 :y 2} "John" {:x 2 :y 1} "Makoto"
   {:x 2 :y 2} "Tony"}
)
;;=> #'chapter03.core/location
(location {:x 2 :y 2})
;;=> "Tony"
```

This is useful when the key is a spatial dimension.

## Using get-in

get-in associates given keys in a vector with a collection and returns a value of the matched element. get-in takes the first argument as a collection and the second argument usually has multiple keys as vectors that you want to look up. We will go back to Conan-Doyle's biography and see how get-in works:

```
(def biography-of-conan-doyle
  {:name "Arthur Ignatius Conan Doyle"
   :born "22-May-1859"
   :died "7-July-1930"
   :occupation ["novelist" "short story writer" "poet" "physician"]
   :nationality "scottish"
   :citizenship "United Kingdom"
   :genre ["Detective fiction", "fantasy", "science fiction", "historical novels", "non-fiction"]
   :notable-works ["Stories of Sherlock Holmes" "The Lost World"]
   :spouse ["Louisa Hawkins" "Jean Leckie"]
   :no-of-children 5
   })
(get-in biography-of-conan-doyle [:genre 2])
;;=> "science fiction"
```

In the preceding example, the second argument of get-in is [:genre 2]. get-in looks for :genre in the var of biography-of-conan-doyle and finds the value as follows:

```
["Detective fiction" "fantasy" "science fiction" "historical novels" "non-fiction"]
```

Then it looks for the third element in the vector and returns "science fiction". The following code is the equivalent code and returns the same result:

```
(get (get biography-of-conan-doyle :genre) 2)
;;=> "science fiction"
```

## Using assoc-in

assoc-in is similar to get-in. It returns a collection that replaces the matched value with the third argument. If there are no matched keys, assoc-in inserts a new value in a new location:

```
(assoc-in {:a {:b 1 :c 2} :d 3} [:a :c] 1)
;;=> {:a {:b 1, :c 1}, :d 3}
(assoc-in {:a {:b 1 :c 2} :d 3} [:a :d] 1)
;;=> {:a {:b 1, :c 2, :d 1}, :d 3}
```

## Using update-in

update-in is similar to get-in, but it only updates existing data. It is also different in that the third argument for update-in is a function.

In the next sample, the first update-in expression increments the matched value. The second expression sets a constant value, 10, using the constantly function:

```
(update-in {:a {:b 1 :c 2} :d 3} [:a :c] inc)
;;=> {:a {:b 1, :c 3}, :d 3}
(update-in {:a {:b 1 :c 2} :d 3} [:a :c] (constantly 10))
;;=> {:a {:b 1, :c 10}, :d 3}
```

## Filtering elements from collections

This section quickly shows how to select only some elements of a Clojure sequence.

## Getting ready

The first parts of the recipe do not need any special preparation, but the final section needs the core.async library to be added to your project.clj file (or any other dependency management you are using):

```
:dependencies [
  [org.clojure/clojure "1.8.0"]
  [org.clojure/core.async "0.2.371"]]
```

## How to do it...

There are four main functions to filter elements:
- filter
- keep and keep-indexed
- remove
- take and take-while

Let's go through a few simple examples.

## Filtering multiples of three

The following code filters numbers that are multiples of three:

```
(filter
  #(= 0 (rem % 3))
  (range 1 10))
; (3 6 9)
```

## Filtering items of a map

This filters keys that are in the map, which is used as a function here:

```
(filter
  {:b 2 :c 3}
  [:a :b])
; (:b)
```

## Filtering non-nil values

This filters non-nil values:

```
(filter #(not (nil? %))
  '(nil 1 2 3))
```

## Removing odd values from a sequence

Here, we generate five random even integers lower than 100. remove will indeed remove all the elements that are odd:

```
(take 5
      (remove
        odd?
        (repeatedly #(rand-int 100))))
; (56 54 86 62 52)
```

## Using keep

keep first applies a function to each item of a collection, and then keeps all the non-nil values :

```
(keep
even?
(range 10 20))
; (true false true false true false true false true false)
```

## Using keep-indexed

keep-indexed is similar to keep, but allows you to put a condition on the index along with the item itself. In the following example, we keep items when the index is even, and then we return the item along a minor transformation. Just like keep, keep-indexed only keeps non-nil values from the function passed as the first parameter:

```
(defn mien [ index item ]
  (if (even? index)
    (inc item)
    nil)
)
```

```
(keep-indexed
  mien
  (range 10 20))
```

## There's more...

Filtering is a very useful Clojure function, that can be used in many different coding situations.

## Filter as a transducer

The `filter` function can also be used as a transducer when no argument is given. We will see that in the next chapter, but here is a quick reminder of how you use this feature.

In the following examples, both `filter` and `map` are used to create a transducer:

```
(transduce
  (filter even?)  conj (range 5))
; [0 2 4]
```

```
(transduce
  (map inc)
  conj
  (range 5))
```

## Filtering with core.async

As of recent times, the `filter` function can be used directly with the channels from the `core.async` library. This makes it very interesting to apply filtering directly at the stream level. This is also done through transducers, as seen here.

We require the library:

```
(require '[clojure.core.async :as async])
```

We create the channel and how to filter odd values that come through it:
