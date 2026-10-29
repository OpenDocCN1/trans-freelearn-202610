```
; ;=> 60
; ;=> nil
```

In the preceding code, using the alter in the dosync blocks other access to the same ref. So, it takes 500 ms * 5 = 2.5 sec to finish. Let's look at the same update using the commute. The commute-add! uses commute instead of alter.

The commute is a function that updates refs inside dosync similar to alter. The difference is that commit using alter is in order, whereas commit using commute is out of order. alter checks the current (updated) value and committing value. If they are the same, the commit takes place. Otherwise there is no committing action and retries. commute does not check the difference when commit takes place. So, let's try the commute version as follows:

```
(defn commute-add! [var val]
  (dosync
    (Thread/sleep 500)
    (commute var  (partial + val))))
; ;=> #'chapter06.concurrency/commute-add!
```

We will measure the performance of updates using commute:

```
(do
  (def v1 (ref 10))
  (time
    (doseq
      [x [(future (commute-add! v1 10))
          (future (commute-add! v1 10))
          (future (commute-add! v1 10))
          (future (commute-add! v1 10))
          (future (commute-add! v1 10))]]
      @x))
    (println @v1)
  )
; ;=> "Elapsed time: 503.967362 msecs"
; ;=> 60
```

The commute version is faster than alter and runs on a highly concurrent environment, but the update operation should be commutative.