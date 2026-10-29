```
(fn[x]
  (= "Friend1"
  (first (:content (parse
  (java.io.StringReader. x))))))
(line-seq in)))
```

This has the drawback of loading more data in memory than needed, but processing is easier to read and concise.

If multiple actions are to be taken while handling the data, transducers could also be added to the mix. We leave that to the curiosity of the reader!