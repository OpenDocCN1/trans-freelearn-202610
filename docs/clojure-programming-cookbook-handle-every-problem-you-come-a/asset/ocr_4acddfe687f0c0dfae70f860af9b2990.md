```
(Thread/sleep 500)
(alter to assoc :amount (+ (:amount @to) amount)))
;;=> #'chapter06.concurrency/refined-transfer!
```

Let's set values of refs using ref-set:

```
(do
  (ref-set makoto-account {:name "Makoto Hashimoto" :amount 1000})
  (ref-set nico-account {:name "Nicolas Modrzyk" :amount 2000}))
;;=> IllegalStateException No transaction running
clojure.lang.LockingTransaction.getEx
(LockingTransaction.java:208)
```

Oops! ref_set should be called inside dosync. Let's try again:

```
(dosync
  (ref-set makoto-account {:name "Makoto Hashimoto" :amount 1000})
  (ref-set nico-account {:name "Nicolas Modrzyk" :amount 2000}))
;;=> "---- ref changed --- " {:name "Nicolas Modrzyk", :amount 2000} " => " {:name "Nicolas Modrzyk", :amount 2000}
;;=> "---- ref changed --- " {:name "Makoto Hashimoto", :amount 1000} " => " {:name "Makoto Hashimoto", :amount 1000}
;;=> {:name "Nicolas Modrzyk", :amount 2000}
```

The updates are successful and the watcher detects the modifications and prints messages. Now, let's test the refined-transfer! function concurrently:

```
(do
  (future
    (refined-transfer! makoto-account nico-account 100))
  (future
    (refined-transfer! nico-account makoto-account 300)))
;;=> #<Future@4c041c9: :pending>
;;=> "---- ref changed --- " {:name "Nicolas Modrzyk", :amount 2000} " => " {:name "Nicolas Modrzyk", :amount 2100}
;;=> "---- ref changed --- " {:name "Makoto Hashimoto", :amount 1000} " => " {:name "Makoto Hashimoto", :amount 900}
;;=> "---- ref changed --- " {:name "Nicolas Modrzyk", :amount 2100} " => " {:name "Nicolas Modrzyk", :amount 1800}
```