one.

Look at the following example:

```
(take 10 (cycle [1 2 3]))
```

Even though we are only accessing the first 10 values, the first 32 will be computed, to improve efficiency.

In `rand-seq`, we do not get stack overflows from `lazy-seq` calls because the realized part of the sequence is being cached, and only the next elements contain information on how to get computed.

# There's more...

Lazy sequences is one of the core key concepts of this book. In this short section, we will see other ways to write lazy sequences, as well as forcing sequences to not be lazy and be realized as soon as possible.

# Other lazy-seq-producing functions

We have seen two functions, `iterate` and `repeatedly`, and the more generic `lazy-seq` macro to generate lazy sequences in a generic way. As much as possible, Clojure sequences are lazy.

For example, the following table shows a list of lazy sequence-producing functions:

| Name | Usage | Example |
|---|---|---|
| `cycle` | Cycles through a sequence or vector and generates an infinite sequence | `(cycle [1 2 3])` |
| `repeat` | Produces an infinite sequence of the same element | `(repeat 1)` |
| `iterate` | As we have seen, produces an infinite sequence of x, (f x) (f (f x)) | `(iterate inc 0)` |
| `range` | Produces an infinite lazy sequence from a starting point | `(range 10)` |