In the following code, the thread is waiting for realizations of x and y by deliver:

```
(future
  (do (deliver z (+ @x @y))
    (println "z value : " @z)))
```

The var z is not still realized:

```
(realized? z)
;;=> false
```

Then we will realize x and y by deliver:

```
(deliver x 1)
;;=> #promise[{:status :ready, :val 1} 0x2f5df579]
(deliver y 1)
;;=> #promise[{:status :ready, :val 1} 0x1614985]
;;=> z value :  2
```

When both values of x and y are set, z is calculated, and thread created by future exits:

```
@z
;;=> 2
```

We will show you another example. We are assuming that multiple products are ready and a shipment instruction has occurred.

In the following code, the Order record has the name, price, and qty properties:

```
(defrecord Order [name price qty])
;; => chapter06.concurrency.Order
```

The merge-products function calculates a total price and merges product info as a vector:

```
(defn merge-products [m1 m2]
  {:total-price
   (+ (* (.price x) (.qty x)) (* (.price y) (.qty y)))}
  [m1 m2])
;; => #'chapter06.concurrency/merge-products
```