

The object itself holds a reference to the first object and to the next function call. The main implemented interface is ISeq. See https://github.com/clojure/clojure/blob/master/src/jvm/clojure/lang/ISeq.j for more details.

## How to do it...

We will progressively see how laziness is put into action, then see how to create our own lazy sequences.

## The iterate function

The iterate function is a simple way to get into lazy sequences. iterate builds a lazy sequence from a function and a starting value. For example, here we build an infinite sequence of integers, starting from zero, adding one at each iteration:

```
(iterate inc 0)
; (0 1 2 3 4 ....
; note the above will hang your REPL, if you don't use
; (set! *print-length* 50)
; to set the max number of items to print from collections.
```

This applies a function, inc, to the first element, 0, which gives 1. 1 is then the new seed of the sequence, and the realized sequence is made up of (0 1). At the next step, we apply inc to the new seed, 1, which gives 2, which is appended to the realized portion of the sequence to give (0 1 2) and the new seed is 2, and so on. The sequence produced will be as follows:

```
((inc 0) (inc (inc 0)) (inc (inc (inc 0))) ...
; (1 (inc 1) (inc (inc 1) ...)
; (1 2 (inc 2))
; and so on
```

We also compute (or we use the term realize), only when needed. Thus, from the full lazy-seq of iterate here, we only take the first five elements:

```
(take 5 (iterate inc 0))
(0 1 2 3 4)
```

## Realized?

To partner with lazy sequences, we have the realized? function, which tells us whether a sequence or subsequence has been evaluated or not. Take the following example, which takes the first five elements of the previous lazy sequence:

```
(def a (take 5 (iterate inc 0)))
; '#a
(realized? a)
; false
(println a)
; console shows: (0 1 2 3 4)
(realized? a)
; true
```

We see that a has the possibility to be computed, but has not been evaluated until we actually refer to its value through the println function. realized? is very useful for checking whether a sequence has been fully computed or not.

## Random sequences with repeatedly

Now, let's say we want to build a sequence of random integers. We similarly use another lazy sequence, producing a function named repeatedly. This function always calls to the same function, and does not depend on a current or previous seed:

```
(repeatedly #(rand-int 10))
; (5 7 2 10 9 2 10 9 8 10 5 0 7 7 10 ...)
```

## Using the macro lazy-seq

To build a more generic lazy sequence, we can use the macro lazy-seq.

From the Clojure lazy page, lazy-seq takes a body of expressions that returns an ISeq or nil, and yields a seqable object that will invoke the body only the first time seq is called, and will cache the result and return it on all subsequent seq calls.

With this in mind, we can re-implement our random int to generate lazy sequences, as follows:

```
(defn rand-seq []
  (lazy-seq
    (cons
      (rand)
      (rand-seq))))
```

As we have seen before, we realize the first element using (rand) and then call upon ourselves again through the same method name (rand-seq).

## How it works...

In Clojure, the lazy-seq macro is implemented directly in Java. There are three key elements in this implementation:

- A head, or a seed, which is the top value of a sequence
- The sequence of realized or computed values
- The information necessary (a function, for example) to compute the other elements of the sequence

Let's see how lazy-seq can be represented with a small diagram:

![](img/d7b631352a782ee50160cf8eb995152f_128_0.png)

A Thunk is enough information for the other elements to be computed.

Note, lazy sequences are evaluated in chunks of around 30 elements, 32 to be precise, so some values are computed ahead of their actual need because this is usually more efficient that making function calls one by one.

Look at the following example:

```
(take 10 (cycle [1 2 3]))
```

Even though we are only accessing the first 10 values, the first 32 will be computed, to improve efficiency.

In `rand-seq`, we do not get stack overflows from `lazy-seq` calls because the realized part of the sequence is being cached, and only the next elements contain information on how to get computed.

## There's more...

Lazy sequences is one of the core key concepts of this book. In this short section, we will see other ways to write lazy sequences, as well as forcing sequences to not be lazy and be realized as soon as possible.

## Other lazy-seq-producing functions

We have seen two functions, `iterate` and `repeatedly`, and the more generic `lazy-seq` macro to generate lazy sequences in a generic way. As much as possible, Clojure sequences are lazy.

For example, the following table shows a list of lazy sequence-producing functions:

| Name | Usage | Example |
|---|---|---|
| `cycle` | Cycles through a sequence or vector and generates an infinite sequence | `(cycle [1 2 3])` |
| `repeat` | Produces an infinite sequence of the same element | `(repeat 1)` |
| `iterate` | As we have seen, produces an infinite sequence of x, (f x) (f (f x)) | `(iterate inc 0)` |
| `range` | Produces an infinite lazy sequence from a starting point | `(range 10)` |
| interleave | Produces a lazy-sequence made of one item of col1, then one item of col2, and so on | (interleave (cycle [:a :b]) (range)) |

Functions in `clojure.core` generate lazy sequences as well. See the following functions:

- `map`
- `filter`
- `remove`
- `take, take-while`
- `drop, drop-last, drop-while`

## Force evaluation with doall

With all this laziness going around, you might wonder how to force evaluation of the lazy sequence ahead of it being accessed. We use a function named `doall`. `doall` forces the realization of all the elements in the lazy sequence. We can check this by calling `realized?` on the sequences, as shown here:

```
(def a
  (take 100 (map inc (cycle [1 2 3]))))
(realized? a)
; false

(def b
  (doall
    (take 100 (map inc (cycle [1 2 3])))))
(realized? b)
; true
```

## Lazy evaluation with files

File reading is a good example of where you should be careful when using lazy sequences, and where it is actually important to pay attention to the order in which the Clojure code is executed. Take this example:

```
(def c  (with-open [rdr (clojure.java.io/reader "project.clj")]
          (interleave (line-seq rdr) (cycle "\n"))))
```

Nothing suspicious at first; you open a file and then use a function named line-seq, which you know produces a lazy sequence of each line of the opened file. interleave then appends an end-of-line character between lines.

So, with all confidence, you rush to the following:

```
(println c)
; and ...
; java.io.IOException: Stream closed
```

What has just happened? The with-open call actually also wraps a close stream call so that by the time the call returns c, the stream has already been closed. Eventually, println asks for the sequence to be realized, but with the stream being closed, things are going wonky.

A quick way to go around the problem is to simply force the execution:

```
(def e (with-open [rdr (clojure.java.io/reader "project.clj")] (apply str (interleave (line-seq rdr) (cycle "\n")))))
```

```
; (def e (with-open [rdr (clojure.java.io/reader "project.clj")] ; (doall (interleave (line-seq rdr) (cycle "\n"))) ))) (println e)
```

This shows that functions that have side effects, such as requiring a state and access to data that is not shown in the code, need to be cautiously used with lazy-seq.

Apart from this particular example, lazy-seq is full steam ahead for efficiency.

## The map function and calling rest

If you remember the map function, you would remember that map, in its simplest form, produces a lazy sequence of a function applied to every element of another sequence:

```
(map inc (range))
```

```
; (1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 ...)
```

map-s gives a nice template to show how to build a lazy sequence using lazy-seq, cons, and rest:

```
(defn map-2
  ([f coll]
    (lazy-seq
      (when-let [s (seq coll)]
        (cons (f (first s)) (map-2 f (rest s)))))))

(map-2 inc (range 0 100))
```

We make use of the following:

- The `lazy-seq` macro, to evaluate the sequence in chunks.
- `cons` concatenates two elements.
- It realizes the first element of the sequence using `first`.
- It calls to itself to keep evaluating the rest of the sequence. `rest` here acts more or less as a pointer to what needs to remain evaluated.

Voila. We have seen how to get our laziness to a new level. No more excuses to not go out and have a great time.

