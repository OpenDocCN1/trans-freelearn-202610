```
(my-concat2 "Hello " "World !")))
;;=> "Elapsed time: 20.369515 msecs"
;;=> nil
```

From the performance aspect, the reflection is not desirable for your application. To check reflections in your application, set *warn-on-reflection* to be true. It warns when reflection occurs:

```
(set! *warn-on-reflection* true)
;;=> true
(def str "This is a String")
;;=> #'performance-example.performance-tips/str
(-> str .toString .toLowerCase)
;;=> Reflection warning, /home/makoto/clojure/clojure-packt-book/chapter09/performance-example/src/performance_example/performance_tips.clj:59:0 - reference to field toString can't be resolved.
;;=> Reflection warning, /home/makoto/clojure/clojure-packt-book/chapter09/performance-example/src/performance_example/performance_tips.clj:59:0 - reference to field toLowerCase can't be resolved.
;;=> "This is a String"
```

In the previous situation, add ^String for str to avoid the warning:

```
(-> ^String str .toUpperCase .toLowerCase)
;;=> "this is a string"
```

Sometimes, setting *warn-on-reflection* always to be true is useful. To do this, add the following to your ~/.lein/profiles.clj_ file:

```
:warn-on-reflection true
```

# Using appropriate data types

Let's compare vector and list. First, we will measure the performance of accessing vectors:

```
(def x (into [] (repeat 1000000 "Clojure Programming Cookbook")))
;;=> #'performance-example.performance-tips/x
(time (first x))
"Elapsed time: 0.093379 msecs"
;;=> "Clojure Programming Cookbook"
```