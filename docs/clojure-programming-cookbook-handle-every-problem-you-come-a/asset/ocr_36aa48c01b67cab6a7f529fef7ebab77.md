# How to do it...

`transduce` is similar to `reduce`, but we will make use of additional transducers. `transduce` turns a collection into another collection through a transducer, then reduces the result to the reducing function.

Following are the two functions, `reduce` and `transduce`, one after the other to compare their usage with + as the reducing function and `(range 5)` as the input collection:

```
(reduce +
    (range 5))
; 10
```

```
(transduce
  (filter odd?)
  +
  (range 5))
; 4
```

The result of the transduce call is indeed very similar to a call for reduce after applying a filter to the original collection:

```
(reduce
  +
  (filter odd? (range 5)))
; 4
```

Except...yes, now is the time to see a bit more how that works under the hood.

# How it works...

Transducers are not so easy to grasp at first, so we will take a bit of time to see what they are made of and then start writing our own, step by step.

# Composable functions

Transducers are first of all composable functions. So, composing a