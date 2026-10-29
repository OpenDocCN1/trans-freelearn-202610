
transducer with another transducer will return a new transducer. One of the most useless transducers you could have will be to `inc` elements first then apply `dec`. Composition of transducers can be done with `comp`:

```
(sequence
  (comp
    (map inc)
    (map dec))
    [1 2 3 4])
; (1 2 3 4)
```

Wow! Great! In this fantastic recipe, the reader can also get to learn how to write code that does literally nothing.

We can see that the resulting transducer used here applies two transformations, one after the other, and just by random luck, it just so happens that they are cancelling each other out.

## Inserting elements

Transducers can also create new elements along the processing. The `interpose` function has a transducer version that can be used to insert generated elements in the resulting sequence:

```
(sequence
  (comp
    (map inc)
    (interpose 1)
  )
  [1 2 3 4])
; (2 1 3 1 4 1 5)
```

So, we also see that the resulting collection can have a very different number of elements from the original collection and that elements can be added or removed along the transducer pipeline.

Let's see how we can write a transducer from scratch that removes elements through processing.

## Writing your own transducers

So, what is the signature of a transducer? Or, more precisely, how do we write one in Clojure code?

A transduce operation can be seen as a pipeline of element-per-element processing. So, to define a transducer, we need the following:

- An init step before any new values have been added to the pipeline
- A step step to process one element of the pipeline
- A last step to complete the processing

Equally, a transducer is a function being understood in the context of a reduction, therefore the reducing function will be a parameter in the creation of that function.

Now, taking all this into account and extracting the important part of the original Clojure filter function, we come up with the following function:

```
(defn my-transfilter [rf]
  (fn
    ; init step
    ([] (rf))

    ; step
    ([result input]
     (if (odd? input)
       (rf result input)
       result))

    ; last step
    ([result] (rf result))
  ))
```

There is quite a bit involved in the preceding code sample, but nothing surprising as such, with the presentation work we have done up to now.

Our transducer is a simple odd-filtering pipeline. The first and last steps of the pipeline are delegating calls directly to the reducing function, and the main step of processing does the extra bit of either filtering and applying the resulting function to the newly coming element, or simply not doing any processing and returning the existing result of the transducing pipeline.

If we try to illustrate this, we could come up with the following:

![](img/d7b631352a782ee50160cf8eb995152f_191_0.png)

In the preceding diagram, the red arrow is the result of applying the reducing function on the result, in blue, and a new coming value from the pipeline, N-1 and N.

Is there a way to stop the pipeline and ask for the end of the pipeline processing you may ask? Yes indeed...

## Short circuit

If you remember, reduce can be short-circuited with reduced. That tells the processing to stop right there and avoid unnecessary computation. The following example stops the processing and ends once a value greater than 5 has been added to the reducing pipeline:

```
(reduce

(fn [result new-value]
(if (< 5 new-value)
(reduced result)
(+ result new-value)))

(range 10))
; 15
```

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

## There's more...

Phew. It has been quite a transducing ride so far, no? But one more pattern is coming along. With transducers being completely context-unaware, we can easily parallelize their use with core.async. Let's quickly see how!

## Transducers with core.async

To play with core.async, we will, just like before, add the dependencies to the project.clj file:

```
:dependencies [
    ...
    [org.clojure/core.async "0.2.385"]
]
```

Then, restart the REPL and start by calling require to add the functions needed to the current namespace:

```
(require '[clojure.core.async :refer (chan buffer pipeline put! take!)])
```

Next comes the meat of the French course with the apply-transducer function, which will create the glue between the in channel and the out channel using the pipeline function from core.async. Pipeline needs a thread-count parameter as well as the transducer function. The rest is now pretty intuitive:

```
(defn apply-transducer
  [in xf]
  (let [
    out (chan 8)
    thread-count 2
    ]
  (pipeline thread-count out xf in)
  out))
```

With this in our suitcase, we can define core.async in and out channels as follows:

```
(def in (chan))
(def out (apply-transducer in (filter odd?)))
```

Now, put some values in the preceding defined channel:

```
(put! in 3)
(take! out (fn [x] (println x)))
; 3 in the console

(put! in 4)
(take! out (fn [x] (println x)))
; nothing printed ... the element has been filtered.
```

This technique can be very useful for handling real-time stream processing, for example, doing encryption or decryption on the fly from Internet of Things devices like Raspberry PI! When doing monitoring, this could also be used to remove irrelevant values or too-noisy values and avoid too much output.

## Summing things up

It has been a long ride, but now we have it. We came all the way from knowing very little about transducers to seeing how they can be created and combined and how they process data through something similar to a pipeline, and, finally, we saw a specifically defined `pipeline` function in `core.async` that can help us apply transducing techniques to real-time stream processing.

Define a transducer that now replays this recipe one more time and reduce itself into a nice cup of coffee.

on core.logic itself:

```
[org.clojure/core.logic "0.8.10"]
```

To check if everything is set up properly, we can already copy and paste our first logic program as follows:

```
(ns chapter03.logic
(:use [clojure.core.logic])
(:require [clojure.core.logic.fd :as fd]))

(run* [q]
    (== q 1))
; (1)
```

Two Clojure imports, and our first goal!!! Find all the values of q that equal to 1. No need for a computer or a book to find the answer. Let's look at the wording used here instead:

- run* tells the logic engine to find all the values.
- q is the goal. There may be intermediate goals in the logic program, but we are only interested in q, which is called a logic var here, or lvar.
- (== q 1) is the constraint. This says to find all the values that are equal to 1.

Our first baby steps; many more coming.

## Getting to know the "o" vocabulary

For some reason, out of scope here, core.logic uses function naming. Apart from the naming, their usage is actually pretty straightforward, so let's review the main ones. We will go quickly through them, so make sure you type them in the REPL and experiment a bit to get accustomed to the language.

## conso: check for vector association

Here we look for a list of items q, such that q results from 1 concatenated to a list of items (2 3):

```
(run* [q]
```

```
(conso 1 [2 3] q)
; ((1 2 3))
```

Note that this is one result, and that result is (1 2 3).

## resto: check for the rest of a vector

Here we look for a var, such that this var equals the rest of vector [1 2 3 4]:

```
(run* [q]
  (resto [1 2 3 4] q))
; ((2 3 4))
```

Note that this is one result, and that result is (2 3 4).

## membero

Keep going on. We want to find a value q, such that 7 is a member of [1 3 8 q]:

```
(run* [q]
  (membero 7 [1 3 8 q]))
; (7)
```

Of course, we can have multiple statements while looking for a result. Let's say we want to find q such that q is a member of [1 2 3] and a member of [2 3 4]. This is pretty much a vector intersection:

```
(run* [q]
  (membero q [1 2 3])
  (membero q [2 3 4]))
; (2 3)
```

That gives us two solutions: q can either be 2 or 3.

Statements can be mixed, of course. Say we want to find members of [1 2 3] that are not 2:

```
(run* [q]
  (!= q 2)
  (membero q [1 2 3]))
; (1 3)
```
