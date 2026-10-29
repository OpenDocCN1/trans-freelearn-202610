tufte is much simpler and no warm-up is necessary. Additionally, it measures multiple expressions at a time and generates comparison reports.

We will use taoensso.tufte/profile and taoensso.tufte/p to run benchmarks and get reports of them. The following is how we make use of profile. Let's assume we'd like to compare three expressions:

```
(tufte/add-basic-println-handler! {})
;;=> #{:basic-println}
(profile {}
  (dotimes [_ 10]
    (p :apply  (apply + (map #(* % %) (range 1 1000001))))
    (p :map  (reduce + (map #(* % %) (range 1 1000001))))
    (p :reducers (r/fold + (r/map #(* % %) (range 1 1000001))))))
;;=>          pId      nCalls      Min        Max        MAD
Mean  Time% Time
;;=>        :map        10     97.0ms   148.02ms   17.27ms
113.45ms    37 1.13s
;;=>        :apply      10     95.26ms  149.86ms   13.86ms
106.6ms     35 1.07s
;;=>  :reducers      10     75.67ms  106.29ms   9.25ms
84.31ms     28 843.07ms
;;=>    Clock Time
100 3.05s
;;=>Accounted Time
100 3.04s
;;=> nil
```

# Using jvisualvm

jvisualvm is included in Oracle's JDK. Using jvisualvm, you can see a lot of JVM activities which Clojure is running. Let's run jvisualvm and connect to your Clojure REPL.

To run jvisualvm, you need to install Oracle's JDK. jvisualvm exists in the $JAVA_HOME/bin. You need to add that path to $PATH and start jvisualvm as follows:

```
$ jvisualvm
```