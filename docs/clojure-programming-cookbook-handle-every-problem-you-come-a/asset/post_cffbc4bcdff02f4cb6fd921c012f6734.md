
```
(defrecord RecordLocation [x y z])
;;=> performance_example.performance_tips.RecordLocation
(deftype TypeLocation [x y z])
;;=> performance_example.performance_tips.TypeLocation
```

Then, we will define map-location, record-location, and type-location:

```
(def map-location {:x 0.0 :y 0.0 :z 0.0})
;;=> #'performance-example.performance-tips/map-location
(def record-location (->RecordLocation 0.0 0.0 0.0))
;;=> #'performance-example.performance-tips/record-location
(def type-location (->TypeLocation 0.0 0.0 0.0))
;;=> #'performance-example.performance-tips/type-location
```

Let's define the ILocation protocol and extend the protocols for map, record, and type in the following:

```
(defprotocol ILocation
  (move [self x y z]))
;;=> ILocation
(extend-protocol ILocation
  clojure.lang.PersistentArrayMap
  (move [self x y z]
    {:x (+ (map-location :x) x)
     :y (+ (map-location :y) y)
     :z (+ (map-location :z) z)})
  RecordLocation
    (move [self x y z]
      (->RecordLocation
        (+ (:x self) x) (+ (:y self) y) (+ (:z self) z)))
  TypeLocation
    (move [self x y z]
      (->TypeLocation
        (+ (.-x self) x) (+ (.-y self) y) (+ (.-z self) z))))
;;=> nil
```

Now, let's measure the performances for them. First, we will test the move method for map:

```
(with-progress-reporting
  (bench    (move map-location 2.0 3.0 2.0)))
```

The result is as follows:

Evaluation count : 386864340 in 60 samples of 6447739 calls.
Execution time mean : 147.959163 ns
Execution time std-deviation : 6.447521 ns
Execution time lower quantile : 139.499802 ns ( 2.5%)
Execution time upper quantile : 162.371977 ns (97.5%)
Overhead used : 9.727097 ns

# Then, we will test for record:

```
(with-progress-reporting
  (bench (move record-location 2.0 3.0 2.0)))
```

# We got the better result:

Evaluation count : 455571960 in 60 samples of 7592866 calls.
Execution time mean : 121.687435 ns
Execution time std-deviation : 3.484611 ns
Execution time lower quantile : 117.866370 ns ( 2.5%)
Execution time upper quantile : 128.063522 ns (97.5%)
Overhead used : 9.727097 ns

# Lastly, we will test the type:

```
(with-progress-reporting
  (bench (move type-location 2.0 3.0 2.0)))
```

# This is better than the previous two:

Evaluation count : 590518980 in 60 samples of 9841983 calls.
Execution time mean : 93.307679 ns
Execution time std-deviation : 2.608460 ns
Execution time lower quantile : 90.346986 ns ( 2.5%)
Execution time upper quantile : 99.742274 ns (97.5%)
Overhead used : 9.727097 ns

# The following table is the summary of benchmarks:

| Data type | Time (ns) |
|-----------|-----------|
| Map       | 147       |
| Record    | 121       |
|           |           |

# Primitive arrays

Let's consider using primitive arrays in Clojure. The following code is to calculate 2 + 2^2 + 3^2 + ... + n ^2 using vector:

```
(def array1 (into [] (repeatedly 1000 #(rand 10))))
;;=> #'performance-example.performance-tips/array1
(defn my-calc1[ar]
  (reduce +
    (map (fn [x] (* x x))
      ar)))
;;=> #'performance-example.performance-tips/my-calc1
```

We will define array2 that is a primitive array of double:

```
(def array2 (double-array (repeatedly 1000 #(rand 10))))
;;=> #'performance-example.performance-tips/array2
```

This is equivalent to the following:

```
double[] array2;
```

Let's check the array is created correctly:

```
(alength ^doubles array2)
;;=> 1000
Try to get the first element.
(aget ^doubles array2 0)
;;=> 7.069889039765091
```

We will define my-calc2 using amap and areduce:

```
(defn my-calc2[ar]
  (let [xs
    (amap ^doubles ar
      idx ret
      (let [x (aget ^doubles ar idx)]
        (* x x)))]
    (areduce xs i ret (double 0)
      (+ ret (aget ^doubles xs i)))))
#'performance-example.performance-tips/my-calc2
```

Let's test the performance of both cases:

```
(bench (my-calc1 array1))
(bench (my-calc2 array2))
Using a primitive type is faster.
Execution time mean : 69.387426 µs
Execution time mean : 54.874642 µs
```

# Working with transient data structures

Clojure's `transient` is a data structure which allows mutable updates. We will learn how we use it and compare the performance with Clojure's persistent data structure.

The following code is using `transient`:

```
(defn rand-seq1 [n m]
  (loop [i 0 s (transient [])]
    (if (< i n)
      (recur (inc i) (conj! s (rand m)))
      (persistent! s))))
```

The `transient` data structure allows mutable functions such as `conj!`, `disj!`, `pop!`, `assoc!`, and `dissoc!`. We cannot use these functions for persistent data structures. To get persistent data, use `persistent!`.

We will define the same function not using `transient`:

```
(defn rand-seq2 [n m]
  (loop [i 0 s []]
    (if (< i n)
      (recur (inc i) (conj s (rand m)))
      s)))
```

Then, let's run benchmarks:

```
(bench (rand-seq1 1000000 10))
(bench (rand-seq2 1000000 10))
```

We got the following results and using a primitive array was faster:

```
transient => Execution time mean : 86.779352 ms
non-transient => Execution time mean : 128.905613 ms
```

# Memoize functions

We introduce you to memoize, which caches the arguments and results of function calls. Memoize improves performance if a function calls with the same arguments repeatedly; on the other hand, the use of memoize consumes memory.

We will define a factorial function in the following:

```
(defn factorial [num]
  (if (= num 1)
    num
    (* num (factorial (dec num))))
```

Then, we will define the memoize version of factorial:

```
(def memoize-factorial (memoize factorial))
```

Now, let's measure the performance of the non-memoize version and the memoize version:

```
(time (factorial 3000N))
;;=> "Elapsed time: 11.111595 msecs"
;;=> 4149359603......
(time (factorial 3000N))
"Elapsed time: 11.260317 msecs"
;;=> 4149359603......
```

We ran the non-memoize version of factorial twice. The execution times are almost the same. Next, we will test the memoize version of factorial:

```
(time (memoize-factorial 3000N))
"Elapsed time: 11.258368 msecs"
;;=> 4149359603......
(time (memoize-factorial 3000N))
"Elapsed time: 0.084887 msecs"
;;=> 4149359603......
```

The first execution is almost the same as the non-memoize version. But the second call is quite fast.

# There's more...

Here, we will show you other tools for profiling performance.

# Logging with timbre and profiling with tufte

ptaoussanis/timbre is a logging API for Clojure/Script. timbre is fast, flexible, and easy to configure with map. ptaoussanis/tufte is a simple profiling and performance monitoring tool for Clojure/Clojurescript.

To enable timbre and tufte, we will add their libraries to your project.clj and restart your REPL:

```
(defproject performance-example "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
            :url "http://www.eclipse.org/legal/epl-v10.html"}
  :dependencies [[org.clojure/clojure "1.8.0"]
                 [criterium "0.4.4"]
                 [com.taoensso/timbre "4.6.0"]
                 [com.taoensso/tufte "1.0.0-RC1"]])
```

Then, let's create src/performance_example/timbre example.clj_. We will add the namespace and library to use:

```
(ns performance-example.timbre-example
  (:require
    [clojure.core.reducers :as r]
    [taoensso.timbre :as timbre
     :refer (log trace debug info warn error fatal report
             logf tracef debugf infof warnf errorf fatallf
             reportf
             spy get-env log-env)]
    [taoensso.tufte :as tufte :refer (defnp p profiled profile)]))
;;=> nil
```

Now, let's test logs:

```
(error "timbre error")
;;=> 16-07-18 14:40:12 phenix ERROR [performance-example.timbre-example:15] - timbre error
;;=> nil
(info "timble info")
```

```
; ;=> 16-07-18 14:40:49 phenix INFO [performance-example.timble-example:18] - timble info
; ;=> nil
```

We have tested two log levels. The next example is to see the expression and the result using spy:

```
(def x 10)
; ;=> #'performance-example.timble-example/x
(spy (* x 10) )
16-07-15 00:06:50 phenix DEBUG [performance-example.timble-example:16] - (* x 10) => 100
; ;=> nil
```

It prints the expression and the value of evaluation. set-level! changes the current log level:

```
(timbre/set-level! :info)
; ;=> {:level :info}
(debug "this is a debug message")
; ;=> nil
```

The previous example sets the log level to info. So the debug log is not printed. Let's set the level back to debug:

```
(timbre/set-level! :debug)
; ;=> {:level :debug}
(debug "this is a debug message")
; ;=> 16-07-18 13:28:03 phenix DEBUG [performance-example.timble-example:30] - this is a debug message
; ;=> nil
```

The debug log is printed. We will define a macro that prints how long an expression takes in milliseconds in the log. We will define ms-time to convert nanoseconds to milliseconds:

```
(defn ms-time[s]
  (double (/ (- (System/nanoTime) s) 1000000)))
; ;=> #'performance-example.timble-example/ms-time
```

Then, we will define a macro for printing time in the log:

```
(defmacro with-perf-log [f s]
  `(let [st# (System/nanoTime)]
```
