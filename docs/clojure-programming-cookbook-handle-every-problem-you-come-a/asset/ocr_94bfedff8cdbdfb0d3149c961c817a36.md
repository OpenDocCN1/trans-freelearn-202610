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