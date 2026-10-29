```
; ;=> "---- ref changed --- " {:name "Makoto Hashimoto", :amount 900} " => " {:name "Makoto Hashimoto", :amount 1200}
```

It works fine!

Let's check in the refined-transfer! whether the control that the balance must not be negative works correctly. The following expression moves 1,500 dollars from Makoto to Nico but Makoto's balance is 1,200 dollars:

```
(refined-transfer! makoto-account nico-account 1500)
; ;=> Exception insufficient bound
chapter06.concurrency/transfer!/fn--20708 (form-init8938610057865502322.clj:87)
```

# Using commute

Let's look at another example using STM. The following function starts a transaction, sleeps 500 ms, and adds value to the ref variable:

```
(defn alter-add! [var val]
  (dosync
    (Thread/sleep 500)
    (alter var  (partial + val))))
; ;=> #'chapter06.concurrency/alter-add!
```

Let's run five threads concurrently and measure how long it takes to finish all updates using the future calls:

```
(do
  (def v1 (ref 10))
  (time
    (doseq
      [x [
        (future (alter-add! v1 10))
        (future (alter-add! v1 10))
        (future (alter-add! v1 10))
        (future (alter-add! v1 10))
        (future (alter-add! v1 10))
      ]]
      @x))
  (println @v1)
)
; ;=> "Elapsed time: 2504.52562 msecs"
```