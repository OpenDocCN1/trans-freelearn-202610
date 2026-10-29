This, by the way, is equivalent to the following:

```
(+ 5 (+ 4 (+ 3 (+ 2 (+ 1 (+))))))
```

We can reuse the same kind of idea when writing our transducer, so a slightly modified and agreed kind of silly version could be as follows:

```
(defn my-transfilter [rf]
  (fn
    ; init
    ([] (rf))
    ; step
    ([result input]
      (reduced 1))
    ; last
    ([result] (rf result))
  ))

(transduce
  my-transfilter
  +
  [ 1 3 8 7])
; 1
```

The processing stops at the first real pipeline processing, and short-circuits all processing to simply return 1. This makes it easy for you to define a way to end the processing of the pipeline on a certain condition straight from the definition of the transducer.

Neat, huh?

# There's more...

Phew. It has been quite a transducing ride so far, no? But one more pattern is coming along. With transducers being completely context-unaware, we can easily parallelize their use with core.async. Let's quickly see how!

# Transducers with core.async

To play with core.async, we will, just like before, add the dependencies