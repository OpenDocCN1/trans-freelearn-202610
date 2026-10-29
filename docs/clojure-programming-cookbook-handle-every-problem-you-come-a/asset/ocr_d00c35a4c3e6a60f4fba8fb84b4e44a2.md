```
[F] ./chapter04.md
[D] ./src
[D] ./src/chapter04
[F] ./src/chapter04/kafka.clj
[F] ./src/chapter04/core.clj
[F] ./src/chapter04/files.clj
[F] ./src/chapter04/rabbitmq.clj
```

# How it works...

Let's look at how `slurp` and `spit` work and then see how `with-open` works.

# slurp and spit

As the `slurp` reads the whole file content into a string, it consumes a lot of memory if the file is very large. Similarly, as the `spit` writes a file from a string one at a time, it consumes a large amount of memory if the target file is very large. In such cases, use reader or writer with sequence read/sequence write instead.

# with-open macro

The `with-open` macro uses `try` and `finally`. Reader or writer is closed by the `close` method in the `finally` clause. The object used in the `with-open` must have `open` and `close` methods. We can understand how the `with-open` works using the `macroexpand-1` as follows:

```
(macroexpand-1
  '(with-open [rdr (clojure.java.io/reader "/etc/passwd")]
    (count (line-seq rdr))
    ))
;;=> (clojure.core/let
;;=>   [rdr (clojure.java.io/reader "/etc/passwd")]
;;=>   (try
;;=>     (clojure.core/with-open [] (count (line-seq rdr)))
;;=>     (finally (. rdr clojure.core/close))))
```

# There's more...

Let's learn how to read web contents via HTTP. Then, we will have a look at a small application to examine Clojure source files.