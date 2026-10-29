named line-seq, which you know produces a lazy sequence of each line of the opened file. interleave then appends an end-of-line character between lines.

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

# The map function and calling rest

If you remember the map function, you would remember that map, in its simplest form, produces a lazy sequence of a function applied to every element of another sequence:

```
(map inc (range))
```