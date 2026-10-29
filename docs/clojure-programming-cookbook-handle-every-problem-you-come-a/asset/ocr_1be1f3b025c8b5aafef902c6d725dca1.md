```
(time (nth x 999999))
;;=> "Elapsed time: 0.157437 msecs"
;;=> "Clojure Programming Cookbook"
(time (x 999999))
;;=> "Elapsed time: 0.134643 msecs"
;;=> "Clojure Programming Cookbook"
(time (last x))
"Clojure Programming Cookbook"
;;=> "Elapsed time: 59.058957 msecs"
;;=> 999999
```

Then, let's test with list:

```
(def y (range 1000000))
;;=> #'performance-example.performance-tips/y
(time (nth y 999999))
;;=> "Elapsed time: 23.200696 msecs"
;;=> "Clojure Programming Cookbook"
(time (last y))
;;=> "Elapsed time: 48.545212 msecs"
;;=> "Clojure Programming Cookbook"
```

Index accesses for vector are faster than those of list. However, both performances of last are not good. So we will define a new last named my-last:

```
(defn my-last [s]
  (nth s (dec (count s))))
;;=> #'performance-example.performance-tips/my-last
(time (my-last x))
;;=> "Elapsed time: 0.210762 msecs"
;;=> "Clojure Programming Cookbook"
(time (my-last y))
;;=> "Elapsed time: 23.701789 msecs"
;;=> "Clojure Programming Cookbook"
```

The previous is better for vector and list.

# Maps versus records versus types

We will test the performance of maps, records, and types. We will define RecordLocation and TypeLocation; those are the records and types as follows: