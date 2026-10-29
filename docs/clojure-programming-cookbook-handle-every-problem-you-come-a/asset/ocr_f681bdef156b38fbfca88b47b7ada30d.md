```
(def read-friends (read-string (pr-str friends)))
(type (first read-friends))
(first read-friends)
```

Similarly, the record types are maintained even after round-tripping through files:

```
(spit "target/text.txt" (pr-str friends))
(read-string (slurp "target/text.txt"))
```

Voila! No more secrets on Clojure data structures and file formats for you now.