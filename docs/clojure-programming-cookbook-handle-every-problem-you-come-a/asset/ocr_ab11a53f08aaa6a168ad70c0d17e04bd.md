Let's test the performance of both cases:

```
(bench (my-calc1 array1))
(bench (my-calc2 array2))
Using a primitive type is faster.
Execution time mean : 69.387426 Âµs
Execution time mean : 54.874642 Âµs
```

# Working with transient data structures

Clojure's `transient` is a data structure which allows mutable updates. We will learn how we use it and compare the performance with Clojure's persistent data structure.

The following code is using `transient`:

```
(defn rand-seq1 [n m]
  (loop [i 0 s (transient [])]
    (if (< i n)
      (recur (inc i) (conj! s (rand m)))
      (persistent! s))))
```

The `transient` data structure allows mutable functions such as `conj!`, `disj!`, `pop!`, `assoc!`, and `dissoc!`. We cannot use these functions for persistent data structures. To get persistent data, use `persistent!`.

We will define the same function not using `transient`:

```
(defn rand-seq2 [n m]
  (loop [i 0 s []]
    (if (< i n)
      (recur (inc i) (conj s (rand m)))
      s)))
```

Then, let's run benchmarks:

```
(bench (rand-seq1 1000000 10))
(bench (rand-seq2 1000000 10))
```

We got the following results and using a primitive array was faster:

```
transient => Execution time mean : 86.779352 ms
non-transient => Execution time mean : 128.905613 ms
```